+++
title = '第一次用Hugo搭建博客的完整流程'
date = 2026-03-17T20:43:58+08:00
draft = true
+++

## 前言

一直想拥有一个属于自己的博客，经过一番研究，最终选择了 **Hugo** 这个静态网站生成器。Hugo 是用 Go 语言编写的，以速度著称，能在毫秒级生成网站。这篇文章记录了我第一次使用 Hugo 搭建博客的完整流程。

---

## 1. 环境准备

### 1.1 安装 Git

前往 [Git 官网](https://git-scm.com/) 下载安装程序，一直点下一步默认安装即可。

### 1.2 安装 Hugo

前往 [Hugo GitHub Tags](https://github.com/gohugoio/hugo/releases) 下载对应版本：

- Windows 用户下载：`hugo_extended_xxxxx_windows_amd64.zip`
- 下载后解压到任意文件夹即可

---

## 2. 创建博客

### 2.1 初始化项目

在 `hugo.exe` 所在文件夹的地址栏输入 `cmd` 回车，打开命令行：

```bash
# 创建 Hugo 站点
hugo new site myblog

# 进入项目目录
cd myblog

# 将 hugo.exe 复制到项目文件夹中（方便后续使用）
```

### 2.2 启动预览服务

```bash
hugo server -D
```

访问 `http://localhost:1313` 即可预览，按 `Ctrl+C` 停止服务。

> 注意：Hugo 默认没有主题，所以页面是空白的，需要配置主题。

---

## 3. 配置主题

### 3.1 选择主题

前往 [Hugo Themes](https://themes.gohugo.io/) 查找喜欢的主题。这里以 **Stack 主题** 为例。

### 3.2 安装主题

1. 下载主题并解压，放到 `/themes` 文件夹中
2. 将 `exampleSite` 中的 `content` 和 `hugo.yaml` 复制到主文件夹
3. 删除 `hugo.toml` 和 `content/post/rich-content`
4. 修改 `hugo.yaml` 中的 `theme` 字段，与主题文件夹同名

### 3.3 再次启动服务

```bash
hugo server -D
```

现在访问 `http://localhost:1313` 就能看到主题效果了！

---

## 4. GitHub 部署

### 4.1 创建仓库

1. 前往 [GitHub](https://github.com) 创建仓库，命名为 `{你的用户名}.github.io`
2. 进入 `Settings -> Pages -> Branch`，选择 `main` 分支并保存
3. 博客地址将是 `https://{你的用户名}.github.io`

### 4.2 生成静态文件

在项目根目录执行：

```bash
hugo -D
```

这会生成 `public` 文件夹，包含所有静态资源。

### 4.3 上传到 GitHub

```bash
cd public
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
git push -u origin main
```

上传成功后，访问 `https://{你的用户名}.github.io` 即可看到你的博客！

---

## 5. GitHub Action 自动部署（推荐）

手动部署比较麻烦，可以使用 GitHub Action 实现自动部署。

### 5.1 创建 Token

1. 前往 `Settings -> Developer Settings -> Personal access tokens`
2. 创建 Classic Token，选择永不过期
3. 勾选 `repo` 和 `workflow` 权限
4. 将 Token 保存到仓库的 `Settings -> Secrets and variables -> Actions` 中

### 5.2 创建工作流

在项目根目录创建 `.github/workflows/deploy.yaml`：

```yaml
name: deploy

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: "latest"
          extended: true

      - name: Build Web
        run: hugo -D

      - name: Deploy Web
        uses: peaceiris/actions-gh-pages@v4
        with:
          PERSONAL_TOKEN: ${{ secrets.你的token变量名 }}
          EXTERNAL_REPOSITORY: 你的github名/你的仓库名
          PUBLISH_BRANCH: main
          PUBLISH_DIR: ./public
          commit_message: auto deploy
```

### 5.3 创建 .gitignore

```
# 自动生成的文件
public
resources
.hugo_build.lock

# hugo命令
hugo.exe
```

### 5.4 推送代码

```bash
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/你的用户名/你的仓库.git
git push -u origin main
```

推送后会自动触发 GitHub Action，完成自动部署！

---

## 总结

通过以上步骤，我成功搭建了自己的 Hugo 博客。整个过程比想象中简单，Hugo 的构建速度非常快，主题也很丰富。最重要的是，使用 GitHub Pages 完全免费！

接下来我会继续探索 Hugo 的更多功能，比如：

- 自定义主题样式
- 添加评论系统
- 配置 SEO
- 添加站点统计

希望这篇文章对想要搭建博客的你有所帮助！
