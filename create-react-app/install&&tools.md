# create-react-app

## 前言

`React`+`antd`的组件库

### 安装

ps:可以使用`npm`,但是`yarn`更快、更安全

    yarn create react-app myproject

初始化脚手架和 React 项目依赖

    cd myproject
    yarn start

浏览器访问[http://localhost:3000](http://localhost:3000)

```
    #项目结构：

    ├── package.json
    ├── public
    | ├── favicon.ico
    | ├── index.html
    | └── manifest.json
    ├── README.md
    ├── src
    | ├── App.css
    | ├── App.js
    | ├── App.test.js
    | ├── index.css
    | ├── index.js
    | ├── logo.svg
    | └── serviceWorker.js
    └── yarn.lock

    #引入antd

    yarn add antd

    #修改`src/App/.css`,在文件顶部引入`antd/dist/antd.css`

    @import '~antd/dist/antd.css';
```

### 文件说明

- `manifest.json`
  对网页做一些配置：如果你打开过某个网页，它会自动有个桌面快捷键，用什么 LOGO，什么内容
- `serviceWorker.js`
  配置好 manifest.json, 使用 registerServiceWorker.js，用户完全可以把你的网页快捷方式放到桌面上，因为你的网页此时支持离线访问，所以用起来和原生 app 的体验很接近。
  **注意只有运行`npm run build`,这个文件才有用**

### sass

    npm install node-sass

### 高级配置

#### react-app-rewired

> 对`create-react-app`的默认配置进行重定义

    yarn add react-app-rewired customize-cra --dev

    /* package.json */
    "scripts": {
    -   "start": "react-scripts start",
    +   "start": "react-app-rewired start",
    -   "build": "react-scripts build",
    +   "build": "react-app-rewired build",
    -   "test": "react-scripts test",
    +   "test": "react-app-rewired test",
    }

在项目的根目录创建一个`config-overrides.js`用于修改默认配置

#### babel-plugin-import

> babel-plugin-import 是一个用于按需加载组件代码和样式的 babel 插件

    yarn add babel-plugin-import --dev

    /*config-overrides.js*/
    + const { override, fixBabelImports } = require      ('customize-cra');
    + module.exports = override(
    +   fixBabelImports('import', {
    +     libraryName: 'antd',
    +     libraryDirectory: 'es',
    +     style: 'css',
    +   }),
    + );

然后移除前面在 src/App.css 里全量添加的 @import '~antd/dist/antd.css';按下面格式引入模块

    import { Button } from 'antd';

**注意：package.json 里的内容要清楚，可以把配置或者工具链划分在`devdependences`中**

### 工具链

- `.editorconfig`
- `prettier`
- `husky + lint-staged + commitlint`

### Redux 初了解

奉上链接：[为啥会有 redux](https://www.zhihu.com/question/41312576/answer/90782136)
