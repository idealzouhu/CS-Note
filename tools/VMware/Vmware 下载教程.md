



# 一 、VMware 下载

## 1.1 VMware Workstation Player 和  VMware Workstation

个人建议**直接下载 VMware Workstation Player 即可**。

VMware Workstation Player 和  VMware Workstation 区别主要在于：

- VMware Workstation Player 免费， VMware Workstation 付费
- VMware Workstation Player 缺失

具体细节查看  [VMware Workstation Player | VMware | CN](https://www.vmware.com/cn/products/workstation-player.html)





## 1.2 VMware Workstation Player 安装







# 二、可能会遇到的问题

VMware Workstation  卸载残留

当存在 VMware Workstation  卸载残留文件时，VMware Workstation Player 无法正常安装

### 解决方案 1 

 直接利用  `VMware_Install_Cleaner.exe` 清除残留文件

VM清理工具链接：https://pan.baidu.com/s/1yLxa0R_JrNj4gmI1YwSNow 提取码：bg4l



### 解决方案 2

查看教程 [blog.csdn.net](https://blog.csdn.net/mrzhang99/article/details/125471301)

①把原有的VM[安装目录](https://www.zhihu.com/search?q=安装目录&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2640717111})下的文件清空。

②把 `C:\Program Files (x86)\Common Files\` 里的 VMware 全部删除。

③Win+R出运行，输入services.msc 调出服务 找到 VM 相关的服务 就是所有带 VMware的。然后，以管理员权限启动 cmd （cmd在C:/windows/system32目录下 从运行-cmd出的是普通权限）运行下列命令，我们可以手动删除服务

```
C:\Windows\System32>sc delete VMAuthdService
[SC] DeleteService 成功

C:\Windows\System32>sc delete VMnetDHCP
[SC] DeleteService 成功

C:\Windows\System32>sc delete VMUSBArbService
[SC] DeleteService 成功
```





