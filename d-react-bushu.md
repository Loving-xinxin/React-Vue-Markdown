## react 部署

<!-- gh-pages -d build -->

- 使用了路由

  - 路由模式是 HashRouter

    - 需要在 package.json 内添加一项 homepage 属性 属性值是自己的服务器地址 例如：`"homepage": "https://Loving-xinxin.io/react-bushu"`
    - 直接执行 npm run build 生成 build 文件夹
    - 将 build 文件夹上传到服务器即可(免费的 git 自己公司的服务器)

  - 路由模式是 BrowserRouter
    由于 BrowserRouter 模式是完全仿照浏览器的历史记录模式，所以如果部署到服务器下的子目录，需要将所有的路由相关地址加上子目录前缀，然后再像 Hash 模式一样生成 build 并上传到服务器。

  - 没有使用路由
    - 直接执行 npm run build 生成 build 文件夹
    - 将 build 文件夹上传到服务器即可(免费的 git 自己公司的服务器)

  使用 ph-pages 简化上传到 git 分支

  - 在项目内安装 gh-pages `npm i gh-pages -D`
  - 在项目下的 package.json 的 script 字段内添加一项 `"deploy":"gh-pages -d build"`
  - 直接在 master 执行 npm run deploy 会直接将 master 分支下的 build 文件内的所有内容直接上传到 gh-pages 分支
