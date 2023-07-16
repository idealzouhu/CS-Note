



## Extension files

Extension 包含不同的文件，具体取决于提供的功能。以下是一些最常用的文件：



## 1.1Manifest

`manifest.json`文件是唯一必需的文件。



- 它必须位于项目的根目录。
- 必须包含的键是`"manifest_version"`、`"name"`和`"version"`。
- 它支持开发过程中的注释（//），但在**将代码上传到ChromeWeb Store之前必须删除这些注释**。



## 1.2 Icons

这些不同大小的`Icons`显示在哪里？

| Icon size | Icon use                                                |
| --------- | ------------------------------------------------------- |
| 16x16     | Favicon on the extension's pages and context menu icon. |
| 32x32     | Windows computers often require this size.              |
| 48x48     | Displays on the Extensions page.                        |
| 128x128   | Displays on installation and in the Chrome Web Store.   |



## 1.3 content scripts







## 1.4  extension project structure











# 参考资料

[Chrome Extensions 101 - Chrome Developers](https://developer.chrome.com/docs/extensions/mv3/getstarted/extensions-101/)