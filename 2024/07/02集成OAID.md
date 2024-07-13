## 集成OAID

https://support.appsflyer.com/hc/en-us/articles/360006278797-Android-OAID-implementation-in-the-SDK

> The Open Anonymous Device Identifier (OAID) is a user-resettable unique identifier for Android devices. It was introduced by the Mobile Security Alliance (MSA), China Information and Communication Research Institute, and device manufacturers, as a privacy-preserving alternative to non-resettable device identifiers like IMEI.
	
	The OAID is usually used for ad measurement and install attribution on devices where Google Play Services aren't available (meaning where Google Advertising ID does not exist).



1. [MSA官网](https://www.msa-alliance.cn)下载OAID SDK

如需SDK下载请提供以下信息，使用公司邮箱发送邮件到msa@caict.ac.cn

- 已注册请注明账号

- 未注册请发送完整公司名称、真实姓名、联系方式、APP名称、加盖公章的营业执照（事业单位法人证书）复印件或扫描件

通过之后会得到一个demo的zip包，解压后，里面有开发者说明文档和FAQ。
注意以下几项：

- oaid_sdk_2.5.0.aar
 ```groovy
implementation files('libs/oaid_sdk_2.5.0.aar')
 ```
- supplierconfig.json
  - 该文件只与vaid的获取有关，如不需获取vaid可不填写，目前只需设置vivo品牌的appid。
  
- example_batch.csv
	- 这个文件跟证书的申请有关，每个包名对应一个签名，申请时把所有需要申请的包名填到里面发送到msa@caict.ac.cn

申请通过后会得到证书文件(应用包名.cert.pem)
	

2. https://github.com/AppsFlyerSDK/appsflyer-oaid

```groovy
implementation 'com.appsflyer:oaid:6.12.3'  
```

集成AF的OAID插件后，就无需开发者写一行代码了。

3. 测试

   安装已集成OAID的包。

   使用AF的原始数据报告，生成报告之前记得勾选OAID。检查导出的原始数据OAID一栏是否有非空项即可。
