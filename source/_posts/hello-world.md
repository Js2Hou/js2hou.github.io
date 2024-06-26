---
title: 项目介绍
date: 2024-06-12 10:17:32
tags: ["快速入门"]
category: "computer science"
---

## 介绍

这是 js2hou 的博客系统，采用 hexo + stellar + github page 搭建。

## 搭建流程

### 安装 nodejs

自行去 nodejs 官网下载安装即可。

### 安装 hexo

[文档 | Hexo](https://hexo.io/zh-cn/docs/)

```bash
npm install -g hexo-cli
```

### 创建项目

直接使用 stellar 提供的模板，克隆仓库 `https://github.com/xaoxuu/hexo-theme-stellar-examples/`，终端进入到 `blog` 文件夹，执行 `npm install`，安装相关依赖。

`_config.yml` 是网站的配置信息，可以在此配置大部分参数。

详细说明见 [配置 | Hexo](https://hexo.io/zh-cn/docs/configuration)。

### github 配置

[在 GitHub Pages 上部署 Hexo | Hexo](https://hexo.io/zh-cn/docs/github-pages)

1. 建立名为 `<github_user_name>.github.io` 的仓库；

2. 将 hexo 文件夹文件 push 到 github，不需要上传依赖，可以参考`.gitignore` 如下：

```bash
node_modules/
public/
```

文件结构不清楚的话，可以参考 [示例存储库](https://github.com/hexojs/hexo-starter)。

3. 查看本机 nodejs 版本，使用指令 `node -v`；

4. 修改 Github 仓库设置，`Setting > Pages > Source`，将 `Source` 修改为 `GitHub Actions`；

5. 仓库中建立文件 `.github/workflows/pages.yml`，并填入一下内容（20 行将nodejs版本替换为自己的）：

```yml
name: Pages

on:
  push:
    branches:
      - main  # default branch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js 20
        uses: actions/setup-node@v4
        with:
          # Examples: 20, 18.19, >=16.20.2, lts/Iron, lts/Hydrogen, *, latest, current, node
          # Ref: https://github.com/actions/setup-node#supported-version-syntax
          node-version: '20'
      - name: Cache NPM dependencies
        uses: actions/cache@v4
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### 访问

等待一小会，随后在 `https://github_user_name.github.io/` 即可访问到自己的博客。

## hexo 常用指令

```bash
# 创建新博客
npx hexo new "post_name"

# 启动服务器：本机 4000 端口
npx hexo server

# 生成静态文件
npx hexo generate

# 部署到远程服务器
npx hexo deploy
```

后三个指令用于本地部署演示，想发布到 github，写完博客直接 push 就可以，GitHub 会自动执行构建。

## 自定义配置

### 自定义头像

需要修改下面几个地方：

1. `_config.stellar.yml` 中，修改 `logo.avatar` 如下所示： 

```yaml
logo:
  avatar: '[/assets/husky.jpg](/about/)' # []() 为 markdown 语法，点击头像跳转到 /about/ 页面
  title: '[Js2Hou](/)'
  subtitle: "Welcome to Js2Hou's world | Coupled with LQ"
```

2. `source/about/index.md` 中，修改 banner 处的 avatar 设置：

```
{% banner Js2Hou 南京炮兵学院的渣渣硕士毕业生，喜欢爬山、徒步、跑步等各种暴汗的运动。 bg:/assets/banner/nebula.jpg avatar:/assets/husky.jpg %}
```

3. `source/_data/wiki/explore.yml` 中，修改 `logo.icon`：

```yaml
logo: 
  icon: /assets/husky.jpg
  title: '[${config.title}](/)'
  subtitle: '探索者的笔记本'
```

### 自定义导航栏

1. `_config.stellar.yml` 中修改 `menubar`，添加或删除内容
   
   ```yml
   menubar:
   items: 
    - id: post
      theme: '#1BCDFC'
      icon: solar:documents-bold-duotone
      title: 博客
      url: /
    - id: topic 
      theme: '#3DC550'
      icon: solar:notebook-bookmark-bold-duotone
      title: 专栏
      url: /topic/
    - id: explore
      theme: '#FA6400'
      icon: solar:planet-bold-duotone
      title: 探索
      url: /explore/
    - id: about
      theme: '#F44336'
      icon: solar:aboutme
      title: 关于我
      url: /about/
   ```

2. 新建文件 `source/{NAME}/index.md`，其中 `{NAME}` 对应步骤1 设置的 url 路径

3. `index.md` 文件修改头部区域，`menu_id` 和 `_config.stellar.yml` 中保持一致

## 参考资料

1. [Stellar：开始您全新的博客之旅 - XAOXUU](https://xaoxuu.com/wiki/stellar/#start)

2. [在 GitHub Pages 上部署 Hexo | Hexo](https://hexo.io/zh-cn/docs/github-pages)
