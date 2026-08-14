<div align="center">
<a href="https://nav.shuichanga.cn"><img height="100px" alt="logo" src="https://nav.shuichanga.cn/img/logo.png"></a>
  <p style="color:blue;font-size:20px;">Simple NAV</p>
  <p><em>自用简约的网址导航站，完全用AI采用Vue框架开发。导航数据通过维基云表格编辑，无需数据库，无需后端，简单且方便。</em></p>
  <div>
    <img src="https://img.shields.io/github/stars/jianzhugo/Simple-Nav?style=flat-square&color=yellow" alt="GitHub Stars">
    <img src="https://img.shields.io/github/forks/jianzhugo/Simple-Nav?style=flat-square&color=green" alt="GitHub Forks">    
    <img src="https://img.shields.io/badge/built%20with-Vue%203-green?style=flat-square" alt="Built with Vue 3">
    <img src="https://img.shields.io/badge/tailwindcss-v2.2.19-blue?style=flat-square" alt="Tailwind CSS">
  </div>
</div>

***

## 功能特点

- [x] 智能本地搜索功能（百度/Bing/谷歌/站内）
- [x] 响应式侧边栏布局，支持折叠/展开
- [x] 双主题系统（默认主题 + 液态玻璃主题），均支持深色模式
- [x] 每个主题独立配置背景（纯色/渐变预设/自定义渐变/背景图片）
- [x] 右下角悬浮工具条（主页/图片预览/主题切换）
- [x] 网站卡片预览图（懒加载 + 请求队列 + 7天缓存）
- [x] 多分类资源管理，自定义分类图标
- [x] 从维基云表格获取数据，无需数据库
- [x] 网站收藏功能，支持快速访问常用网站
- [x] 直接增加/编辑/删除网址，密码保护
- [x] 配置导入导出功能，支持选择导入导出项
- [x] 自定义背景颜色/渐变/图片/卡片列数

## 演示站

<https://nav.shuichanga.cn>

开户预览图
![开启](https://nav.shuichanga.cn/img/kaiqi.webp)

关闭预览图
![关闭](https://nav.shuichanga.cn/img/guanbi.webp)

## 维基云表格

[表格链接](https://vika.cn/share/shrxaWuBbbn6cKWBwvXgV/dstfGaY66aN2wyLlmX/viwullRf3ubdS)

![表格](https://nav.shuichanga.cn/img/demo2.png)

## 安装部署


> **关于 SPA 路由回退**
>
> 项目使用 Vue Router 的 History 模式，直接访问 `/settings`、`/about`、`/search` 等子路径刷新时，需要服务器把这些路径回退到 `index.html` 交给前端路由处理。
>
> 项目已在 [`public/_redirects`](./public/_redirects) 中内置回退规则：
>
> ```
> /*    /index.html   200
> ```
>
> Cloudflare Pages 与腾讯 EdgeOne Pages 均会自动识别此文件，**无需额外配置**。其它静态托管平台（如 Vercel、Netlify）也兼容此约定。

### 方案一：使用腾讯 EdgeOne Pages 部署

我自己是使用的腾讯 EdgeOne Pages 部署的，免费快速。

1、Fork项目（<https://github.com/jianzhugo/Simple-Nav）>

2、打开下方 EdgeOne Pages 地址

[Pages地址](https://console.cloud.tencent.com/edgeone/pages)

3、点击创建项目，选择"导入Git仓库",选择刚才Fork的仓库

4、在构建部署配置中按下图填写，然后点完成就可以了。

![配置](https://nav.shuichanga.cn/img/demo3.png)

5、在项目设置中，将"环境变量"中的"VITE\_API\_PASSWORD"设置为你自己的密码。（用于直接增加网址功能）
![获取](https://nav.shuichanga.cn/img/mima.png)

### 方案二：使用 Cloudflare Pages 部署

[Cloudflare Pages](https://pages.cloudflare.com/) 提供免费的静态站点托管，自带全球 CDN，部署本项目步骤如下。

#### 一、Fork 项目

Fork 仓库：<https://github.com/jianzhugo/Simple-Nav>

#### 二、连接 Git 仓库

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)，左侧选择 **Workers & Pages**。
2. 点击 **Create application** → **Pages** → **Connect to Git**。
3. 授权 Cloudflare 访问你的 GitHub 账号，选择刚才 Fork 的仓库。

#### 三、填写构建配置

| 配置项 | 值 |
| --- | --- |
| Framework preset | `Vite` |
| Build command | `npm run build` |
| Build output directory | `dist` |
| Root directory | （留空） |

#### 四、配置环境变量

在 **Settings → Environment variables** 中添加以下变量（Production 和 Preview 环境都建议配置）：

| 变量名 | 说明 |
| --- | --- |
| `VITE_VIKA_API_KEY` | 维格云 API 密钥 |
| `VITE_VIKA_DATASHEET_ID` | 维格云表格 ID |
| `VITE_VIKA_VIEW_ID` | 维格云视图 ID |
| `VITE_SUBMIT_PASSWORD` | 右上角直接增加网址功能的密码 |

> ⚠️ **安全提示**：以 `VITE_` 前缀开头的变量会在构建时被打包进前端 JS，任何访问者都能在浏览器源码中看到。建议只填写**只读权限**的维格云 Token；如需隐藏写操作 Key，可改用 Cloudflare Pages Functions 做一层 API 代理（需自行改造）。

#### 五、部署

点击 **Save and Deploy**，等待构建完成即可获得 `xxx.pages.dev` 的访问域名。

后续每次 push 到默认分支会自动触发部署；也可绑定自定义域名（**Custom domains** 中添加，Cloudflare 会自动签发 SSL 证书）。

#### 六、SPA 路由（已内置，无需操作）

项目根目录的 `public/_redirects` 会在构建时被拷贝到 `dist/_redirects`，Cloudflare Pages 会自动识别并应用其中的回退规则，访问 `/settings`、`/about`、`/search` 直接刷新不会出现 404。

#### 可选：使用 Wrangler 命令行部署

如不想绑定 Git，可本地构建后用 `wrangler` 直接上传：

```bash
# 安装 wrangler
npm install -g wrangler

# 登录 Cloudflare
wrangler login

# 本地构建
npm run build

# 上传部署
wrangler pages deploy dist
```

### 改数据

1、在维基云注册账号，新建表格。表格格式如下

![格式](https://nav.shuichanga.cn/img/demo4.png)

2、获取表格的APIkey、datasheetId、viewId

![获取](https://nav.shuichanga.cn/img/demo7.png)

### 配置方法

#### 方法一：通过设置界面配置

在设置中填入相应的APIkey、datasheetId、viewId。

![api](https://cdn.jsdmirror.com/gh/jianzhugo/image01/20251217130610676.png)

#### 方法二：通过环境变量配置

可以通过创建 `.env` 文件或在部署平台设置环境变量来配置，变量名如下：

- `VITE_VIKA_API_KEY`：维格云 API 密钥
- `VITE_VIKA_DATASHEET_ID`：维格云表格 ID
- `VITE_VIKA_VIEW_ID`：维格云视图 ID
- `VITE_SUBMIT_PASSWORD`：右上角直接增加网址功能的密码

**配置示例**（.env 文件）：

```env
VITE_SUBMIT_PASSWORD=你的密码
VITE_VIKA_API_KEY=your_api_key_here
VITE_VIKA_DATASHEET_ID=your_datasheet_id_here
VITE_VIKA_VIEW_ID=your_view_id_here
```

**优先级说明**：环境变量配置优先于设置界面配置。如果同时设置了环境变量和界面配置，系统会优先使用环境变量的值。

3、自定义分类图标

在设置中自定义分类图标。

![图标映射](https://cdn.jsdmirror.com/gh/jianzhugo/image01/20251217130740909.png)

4、配置导入导出

在设置中可以导入导出配置，方便在不同电脑间使用相同配置。

- 可以选择要导入导出的配置项
- 导出配置会生成JSON文件，包含所选配置项
- 导入配置支持从JSON文件读取配置并应用
- 默认不导出API配置，保护敏感信息

## 直接增加网址

点击右上角的"+"号，输入网址和分类，即可直接增加网址。（需验证密码）
![api](https://nav.shuichanga.cn/img/zengjiawangzhi.png)

## 技术栈

- **Vue 3** — Options API
- **Vue Router** — History 模式
- **Tailwind CSS** — 原子化样式
- **Font Awesome** — 图标库
- **Vite** — 构建工具
- **维基云 (Vika)** — 数据源 API

## 其它

根据自己需要修改关于页面及其它界面代码。
