---
title: czy1121_update应用笔记(一）——搭建服务
date: 2018-1-16 18:49:27
tags: [update]
categories: 第三方库
---
最近做的一个APP需要检测更新，以前都是自己写的，这次决定使用别人封装好的一个库。

[czy1121/update](https://github.com/czy1121/update)
清晰灵活简单易用的应用更新库

第一步，需要搭建一个检测更新的服务。这里直接在之前写的一个库的基础上修改。

[chenzhenlindx/SpringBootDemo](https://github.com/chenzhenlindx/SpringBootDemo)
清晰灵活简单易用的应用更新库
首先导入此项目，或者按照说明创建项目。

要实现的效果为：
``` bash
url：http://example.com/check?package=ezy.demo.update&version=123&channel=yyb
method：get
response：
{
	"hasUpdate": true,
	"isSilent": false,
	"isForce": false,
	"isAutoInstall": true,
	"isIgnorable": true,
	"versionCode": 2,
	"versionName": "v2.0.2018-1-16",
	"updateContent": "1、修改了bug1；\r\n2、修改了bug2；\r\n3、新增了某个功能。",
	"url": "http://10.129.51.27:8080/apk/apk2018-1-15.apk",
	"md5": "34db53f031fccf92b779e0d160ef37c1",
	"size": 1824234
}
```
1、在package com.example.demo.domain创建UpdateInfo.java
``` bash
public class UpdateInfo {
	// 是否有新版本
	public boolean hasUpdate = false;
	// 是否静默下载：有新版本时不提示直接下载
	public boolean isSilent = false;
	// 是否强制安装：不安装无法使用app
	public boolean isForce = false;
	// 是否下载完成后自动安装
	public boolean isAutoInstall = true;
	// 是否可忽略该版本
	public boolean isIgnorable = true;

	public int versionCode;
	public String versionName;
	public String updateContent;

	public String url;
	public String md5;
	public long size;
}
```
2、在resources目录下创建static/apk文件夹，将新版本apk复制到此目录
![](/images/UpdateDemo/apk_path.png)
启动服务后，点击 http://10.129.51.27:8080/apk/apk2018-1-15.apk 弹窗文件下载框，表示文件位置正确。
![](/images/UpdateDemo/apk_dialog.png)

3、创建AppConttroller.java
``` bash
package com.example.demo.controller;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.math.BigInteger;
import java.security.MessageDigest;

import org.springframework.util.ResourceUtils;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import com.example.demo.domain.UpdateInfo;

@RequestMapping("/app")
@RestController
public class AppConttroller {
	/**
	 * http://10.129.51.27:8080/app/check?package=cn.czl.updatedemo&version=1&channel=main
	 * 
	 * @return
	 * @throws FileNotFoundException
	 */
	@RequestMapping(value = "/check", method = RequestMethod.GET)
	public UpdateInfo check(@RequestParam(name = "package", required = true) String pkg,
			@RequestParam(name = "version", required = true) int version,
			@RequestParam(name = "channel", required = true) String channel) throws FileNotFoundException {
		UpdateInfo updateInfo = new UpdateInfo();
		if (version > 1) {
			updateInfo.hasUpdate = false;
		} else {
			updateInfo.hasUpdate = true;
			updateInfo.versionCode = 2;
			updateInfo.versionName = "v2.0.2018-1-16";
			updateInfo.updateContent = "1、修改了bug1；\r\n" + "2、修改了bug2；\r\n" + "3、新增了某个功能。";

			updateInfo.url = "http://10.129.51.27:8080/apk/apk2018-1-15.apk";
			File file = ResourceUtils.getFile("classpath:static/apk/apk2018-1-15.apk");
			updateInfo.md5 = md5(file);
			updateInfo.size = file.length();
		}
		return updateInfo;
	}

	public static String md5(File file) {
		MessageDigest digest = null;
		FileInputStream fis = null;
		byte[] buffer = new byte[1024];
		try {
			if (!file.isFile()) {
				return "";
			}
			digest = MessageDigest.getInstance("MD5");
			fis = new FileInputStream(file);
			while (true) {
				int len;
				if ((len = fis.read(buffer, 0, 1024)) == -1) {
					fis.close();
					break;
				}
				digest.update(buffer, 0, len);
			}
		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}
		BigInteger var5 = new BigInteger(1, digest.digest());
		return String.format("%1$032x", new Object[] { var5 });
	}
}

```

在浏览器访问：
http://10.129.51.27:8080/app/check?package=cn.czl.updatedemo&version=1&channel=main
即可得到预想的json。
注意：
1. 正常情况我们应该apk使用表单上传，更新信息应该从表单获取；
2. 代码直接当version>1判断无更新，实际上应该查询数据库最新版本进行比较；
3. md5是获取apk文件的md5，而不是打包签名文件的md5，具体的md5获取代码如上。

至此，服务端代码编写完成，详情可参考
[czy1121Update/SpringBoot](https://github.com/chenzhenlindx/czy1121Update/tree/master/SpringBoot)
  