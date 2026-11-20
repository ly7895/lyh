# 待办清单 PWA

一个可以在安卓手机上「添加到主屏幕」、像 App 一样全屏打开的待办清单。

## 文件说明

| 文件 | 作用 |
|---|---|
| `index.html` | 整个 App（界面 + 逻辑都在这一个文件里） |
| `manifest.json` | PWA 安装信息：App 名称、图标、启动方式（全屏） |
| `sw.js` | Service Worker，负责离线缓存，断网也能打开 |
| `icon-192.png` / `icon-512.png` | App 图标（普通） |
| `icon-512-maskable.png` | App 图标（安卓自适应，防止被裁成圆形时变形） |
| `icon.svg` | 矢量图标源文件 |
| `make_icons.py` | 重新生成 PNG 图标的脚本（改了设计跑一次即可） |

## 功能（第一版）

- ✅ 添加任务（输入框 + 回车或点 +）
- ✅ 勾选完成 / 取消完成
- ✅ 删除单条任务
- ✅ 清除全部已完成
- ✅ 数据存在手机本地（localStorage），关掉再开不丢
- ✅ 跟随系统的深色模式
- ✅ 离线可用

## 本地预览

在 `todo-app` 目录下起一个静态服务器（PWA 必须走 http，不能直接双击 html 文件）：

```powershell
python -m http.server 8000 --bind 0.0.0.0
```

电脑访问 `http://127.0.0.1:8000`，手机连同一个 Wi-Fi 访问 `http://<电脑局域网IP>:8000`。

> 注意：局域网 IP 是 http 而非 https，Chrome 可能不显示「安装应用」按钮，
> 这时可以先在浏览器里正常使用，正式装到桌面请用部署后的 https 地址。

## 部署到公网（正式使用）

任选其一，都是免费且自带 https：

**方案 A：Vercel（推荐）**
1. 注册 [vercel.com](https://vercel.com)，用 GitHub 登录
2. 新建项目，上传或连接这个 `todo-app` 目录
3. Framework Preset 选 `Other`，Build Command 留空，Output Directory 填 `.`
4. 部署后会得到一个 `https://xxx.vercel.app` 地址

**方案 B：Netlify**
1. 打开 [app.netlify.com/drop](https://app.netlify.com/drop)
2. 直接把 `todo-app` 文件夹拖进去，立刻得到一个 https 地址

**方案 C：GitHub Pages**
1. 新建仓库，上传这些文件
2. Settings → Pages → Source 选 `main` 分支 `/ (root)`
3. 地址是 `https://用户名.github.io/仓库名/`

## 安装到安卓手机桌面

1. 手机 **Chrome** 打开部署后的 https 地址
2. 右上角 `⋮` 菜单 → **「安装应用」**（部分版本显示「添加到主屏幕」）
3. 确认后桌面出现蓝色清单图标，点开即全屏运行，没有地址栏

用 Edge 同理：菜单 → 「添加到手机」。

> 微信内置浏览器不支持安装到桌面，必须用 Chrome / Edge 打开。

## 以后想加功能

数据存的是这个结构，加字段不会影响老数据：

```js
{ id: "唯一ID", text: "任务内容", done: false, createdAt: 1735000000000 }
```

升级 `sw.js` 里的 `CACHE` 版本号（比如改成 `todo-app-v2`），手机才会拉到新的缓存。
