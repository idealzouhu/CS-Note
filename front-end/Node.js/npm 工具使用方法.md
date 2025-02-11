### npm install

用途：用于安装项目依赖。
执行过程：

1. 读取 package.json 文件中的 dependencies 和 devDependencies 字段。
2. 下载并安装列出的所有依赖包到项目的 node_modules 目录中。

常见场景：

- 初次克隆项目后，需要安装所有依赖包。
- 更新依赖包版本后，重新安装以确保使用最新版本。



### npm run serve

用途：启动开发服务器，通常用于前端项目的开发环境。
执行过程：

1. 执行 package.json 中定义的 scripts 字段下的 serve 脚本。
2. 启动一个本地开发服务器（如 Vue CLI、Create React App 等工具提供的开发服务器），提供热重载、错误覆盖等功能。

常见场景：

- 在开发过程中实时预览和调试代码。
- 自动编译和刷新页面，提高开发效率。







### rimraf

```
$ npm install rimraf@5.0.0 -g
D:\Enviroment\frontend\nodejs-14.21.3\node_global\rimraf -> D:\Enviroment\frontend\nodejs-14.21.3\node_global\node_modules\rimraf\dist\cjs\src\bin.js
+ rimraf@5.0.0
added 1 package and updated 6 packages in 1.925s
```

