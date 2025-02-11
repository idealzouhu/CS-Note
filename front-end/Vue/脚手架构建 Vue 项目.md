

### 初始化项目







### 项目结构

- `package.json`：该文件包含项目的依赖项列表，以及一些元数据和 `eslint` 配置。

- `yarn.lock`：如果你选择 `yarn` 作为你的包管理器，将生成此文件，其中包含项目所需的所有依赖项和子依赖项的列表。

- `babel.config.js`：这个是 [Babel](https://babeljs.io/) 的配置文件，可以在开发中使用 JavaScript 的新特性，并且将其转换为在生产环境中可以跨浏览器运行的旧语法代码。你也可以在这个里配置额外的 babel 插件。

- `jsconfig.json`：这是一份用于 [Visual Studio Code](https://code.visualstudio.com/docs/languages/jsconfig) 的配置文件，它为 VS Code 提供了关于项目结构的上下文信息，并帮助自动完成。

- `public`：这个目录包含一些在 [Webpack](https://webpack.js.org/) 编译过程中没有加工处理过的文件（有一个例外：index.html 会有一些处理）。

  - `favicon.ico`：这个是项目的图标，当前就是一个 Vue 的 logo。

  - `index.html`：这是应用的模板文件，Vue 应用会通过这个 HTML 页面来运行，也可以通过 lodash 这种模板语法在这个文件里插值。

    **备注：**这个不是负责管理页面最终展示的模板，而是管理 Vue 应用之外的静态 HTML 文件，一般只有在用到一些高级功能的时候才会修改这个文件。

- `src`：这个是 Vue 应用的核心代码目录

  - `main.js`：这是应用的入口文件。目前它会初始化 Vue 应用并且制定将应用挂载到 `index.html` 文件中的哪个 HTML 元素上。通常还会做一些注册全局组件或者添额外的 Vue 库的操作。
  - `App.vue`：这是 Vue 应用的根节点组件，往下看可以了解更多关注 Vue 组件的信息。
  - `components`：这是用来存放自定义组件的目录，目前里面会有一个示例组件。
  - `assets`：这个目录用来存放像 CSS、图片这种静态资源，但是因为它们属于代码目录下，所以可以用 webpack 来操作和处理。意思就是你可以使用一些预处理比如 [Sass/SCSS](https://sass-lang.com/) 或者 [Stylus](https://stylus-lang.com/)。



```
my-vue-project/
├── node_modules/                  # 项目依赖（自动生成）
├── public/                        # 静态文件（通常不需要 Webpack 处理）
│   ├── index.html                 # 入口 HTML 文件
│   └── favicon.ico                # 网站图标
├── src/                           # 源代码
│   ├── assets/                    # 静态资源（图片、样式、字体等）
│   │   ├── logo.png
│   │   └── styles.css
│   ├── components/                # Vue 组件
│   │   ├── Header.vue
│   │   ├── Footer.vue
│   │   └── MyComponent.vue
│   ├── directives/                # 自定义指令（可选）
│   │   └── v-focus.js
│   ├── router/                    # 路由配置
│   │   └── index.js               # 路由实例
│   ├── store/                     # 状态管理（Vuex）
│   │   ├── index.js               # Vuex store 配置
│   │   └── modules/               # Vuex modules
│   ├── views/                     # 视图组件（通常是页面级别的组件）
│   │   ├── Home.vue
│   │   └── About.vue
│   ├── App.vue                    # 根组件
│   ├── main.js                    # 入口 JavaScript 文件
│   └── styles/                    # 全局样式
│       └── global.css
├── tests/                         # 测试代码（单元测试、E2E 测试）
│   ├── unit/                      # 单元测试
│   │   ├── App.spec.js
│   │   └── MyComponent.spec.js
│   └── e2e/                       # 端到端测试
│       └── test.spec.js
├── .gitignore                     # Git 忽略文件
├── babel.config.js                # Babel 配置
├── eslint.config.js               # ESLint 配置
├── jsconfig.json                  # JavaScript 配置（VS Code 支持）
├── package.json                   # 项目依赖和脚本配置
├── README.md                      # 项目说明文档
├── vue.config.js                  # Vue CLI 配置（如果使用 Vue CLI）
└── vite.config.js                 # Vite 配置（如果使用 Vite）
```





### 参考资料

[开始使用 Vue - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Vue_getting_started)