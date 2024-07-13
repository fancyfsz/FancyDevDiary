### **Guideline 4.2.3 - Design - Minimum Functionality**

We were required to install the WeChat app before we could log in to your app. Users should be able to log in and access their accounts without having to install any additional apps.

**Next Steps**

If you would like to offer authentication through WeChat, please use a mechanism that allows users to log in from within your app without first having to install an additional app.

We recommend implementing the Safari View Controller API to display web content within your app. The Safari View Controller allows the display of a URL and inspection of the certificate from an embedded browser in an app so that customers can verify the webpage URL and SSL certificate to confirm they are entering their sign in credentials into a legitimate page.

**Resources**

For additional information on the Safari View Controller API, please review the [What's New in Safari](https://developer.apple.com/library/archive/releasenotes/General/WhatsNewInSafari/Introduction/Introduction.html) webpage.

**Support**

\- Reply to this message in your preferred language if you need assistance. If you need additional support, use the [Contact Us module](https://developer.apple.com/contact/topic/#!/topic/select).
\- Consult with fellow developers and Apple engineers on the [Apple Developer Forums](https://developer.apple.com/forums/).
\- Help improve the review process or identify a need for clarity in our policies by [suggesting guideline changes](https://developer.apple.com/contact/app-store/?topic=guideline).



解读：苹果不允许玩家需要安装任何第三方App来完成登陆操作。

解决：可以检测玩家是否安装了所需第三方App，如若没有安装隐藏入口即可，或者提供网页版的登陆或其他授权方式。