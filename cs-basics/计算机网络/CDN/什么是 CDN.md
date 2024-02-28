# 一、CDN 概述

## 1.1 为什么提出 CDN

伴随着快速发展的互联网流量，网络拥塞将成为互联网发展的一大难题。

 Tim Berners-Lee 提出一种革新性的互联网服务-- CDN，以此希望<font color="red">**解决互联网内容的无拥塞分发**</font>



## 1.2 什么是 CDN

CDN （Content Delivery Network, 内容分发网络）相关定义：

- 在 [阿里云CDN](https://help.aliyun.com/zh/cdn/product-overview/what-is-alibaba-cloud-cdn?spm=a2c4g.11174283.0.0.78ac64c5GyQajG) 的介绍里, CDN是建立并覆盖在承载网之上，由遍布全球的边缘节点服务器群组成的分布式网络。

- 简单来说，CDN 是一种利用分布式节点和缓存技术，将内容（如网页、图片、视频等）缓存到离用户较近的服务器上，以提高用户访问这些内容的速度和稳定性的技术。

CDN能分担源站压力，避免网络拥塞，确保在不同区域、不同场景下加速网站内容的分发，**提高资源访问速度。**



## 1.3 常见的 CDN 服务

[阿里云CDN](https://help.aliyun.com/zh/cdn/product-overview/what-is-alibaba-cloud-cdn?spm=a2c4g.11174283.0.0.78ac64c5GyQajG)





# 二、CDN 工作原理



## 相关概念

CNAME 是 DNS 解析里面的一个概念，相关资料查看 [简单的解释下什么是CNAME？-CSDN博客](https://blog.csdn.net/DD_orz/article/details/100034049)





## CDN 工作流程

![img](images/ciawk92i4h.png)



[阿里云CDN_工作流程](https://help.aliyun.com/zh/cdn/product-overview/what-is-alibaba-cloud-cdn?spm=a2c4g.11186623.0.0.f0f27ec5CChIls#section-wql-p2l-56f)

![原理](images/p352419.png)



> Local DNS服务器怎么知道一个域名的授权（权威）服务器是哪台？这个域名应该在哪里取解析呢？
> 首先公司会去找运营商买域名，比如 CDN公司买了cdn.com这个一级域名（相对于a.cdn.com和b.cdn.com来说cdn.com可以称为一级域名，这个不用纠结哈），那么本地运营商会做一个NS记录，即匹配到这个cdn.com后缀的域名都会到CDN服务提供商的DNS服务器做解析，即到权威服务器做解析。



# 三、应用场景

[应用场景_CDN(CDN)-阿里云帮助中心 (aliyun.com)](https://help.aliyun.com/zh/cdn/product-overview/scenarios?spm=a2c4g.11186623.0.0.27415a3bqi5FHU)





# 参考资料

[什么是CDN？它解决了什么难题？5分钟让你明明白白！-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1779335)

[CDN是什么?用了一定更快吗？（上）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ez421d7TC/?spm_id_from=333.1007.tianma.3-2-8.click&vd_source=52cd9a9deff2e511c87ff028e3bb01d2)