---
author_name: Adrian Hall (MSFT)
layout: post
hide_excerpt: true
---
      [Adrian Hall (MSFT)](https://social.msdn.microsoft.com/profile/Adrian Hall (MSFT))  7/18/2016 9:00:26 AM  Apple announced in September 2015 that from the 1st of June, all apps submitted to the iOS App Store for review must [support IPv6 only networks](https://developer.apple.com/news/?id=05042016a). **TL;DR** – your app submissions will likely work just fine as long as you are not embedding IPv4 addresses or using IPv4 specific communication methods in your apps. For more information, keep reading. ### Why IPv6

 IPv4 address are exhausted, and carriers are deploying IPv6 only which make it critical that our apps continue to work on IPv6 only networks. Fortunately for us, most apps will continue to work on an IPv6 only network without modification. To ensure your apps continue to work on an IPv6 only work, you'll want to make sure you're using a network library (for example [NSUrlSession](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSURLSession_class/index.html) or [CFNetwork](https://developer.apple.com/library/mac/documentation/Networking/Conceptual/CFNetwork/Introduction/Introduction.html)). You'll also want to avoid the use of IPv4 APIs and hard-coded IP addresses. Essentially if your app is using a higher level API and framework for the network rather than the IP layer, your app should continue to work on top of either IPv4 or IPv6. For more information, check out the [Apple Documentation](https://developer.apple.com/library/ios/documentation/NetworkingInternetWeb/Conceptual/NetworkingOverview/UnderstandingandPreparingfortheIPv6Transition/UnderstandingandPreparingfortheIPv6Transition.html). [![ipv6]({{ site.baseurl }}/media/2016/07/ipv6-500x272.jpg)]({{ site.baseurl }}/media/2016/07/ipv6.jpg) As part of the carriers upgrade to IPv6, they have deployed both [DNS64 and NAT64](https://en.wikipedia.org/wiki/IPv6_transition_mechanism) on their network which allows IPv6 clients (your app) to communicate with existing IPv4 infrastructure. If we take an App Service running in Azure as an example, it currently supports IPv4, but we still wish to communicate with it on the IPv6 carrier network. When our iOS app attempts to contact the REST endpoint we have set up, it will use a host name rather than a hard-coded IPv4 address. The DNS64 server will synthesize an IPv6 address and pass this back to the client. Any packets get routed to the NAT64 engine, which will then translate IPv6 traffic to IPv4 and vice versa. Through using DNS64 and NAT64, our IPv4 only App Service now appears (to our connecting client) to be an IPv6 service. ### Testing

 If you wish to test if your app supports IPv6 only networks without submitting your app for review, you can use [a handy addition to MacOS network settings](https://developer.apple.com/library/ios/documentation/NetworkingInternetWeb/Conceptual/NetworkingOverview/UnderstandingandPreparingfortheIPv6Transition/UnderstandingandPreparingfortheIPv6Transition.html#//apple_ref/doc/uid/TP40010220-CH213-SW16). Apple have provided a DNS64 and NAT64 implementation in MacOS 10.11. This can be enabled by option clicking on the **Share** pane in **System Preferences** and then option-clicking the Internet Sharing service. After that, you'll see the Create NAT64 Network option appears. [![sysprefs]({{ site.baseurl }}/media/2016/07/sysprefs-425x350.png)]({{ site.baseurl }}/media/2016/07/sysprefs.png) ### Find out More

 iOS developers should take the time to watch [Apple's video from WWDC](https://developer.apple.com/videos/play/wwdc2015/719/) in order to learn more about the new app store requirement to support IPv6. Most developers will not experience any issues with the new requirement as long as they've followed best practices; even if their existing infrastructure has no support for IPv6.      