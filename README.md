# 六爻排盘 · GitHub Pages 双版本

一个纯静态的六爻排盘工具，打包成 iOS 与 Android 两个版本，可直接托管在 GitHub Pages 上。
两版功能完全一致，差异只在「装到桌面」的系统适配。

```
github/
├── index.html              ← 版本选择页（自动识别手机系统）
├── ios/                    ← iPhone / iPad 版
│   ├── index.html
│   ├── manifest.webmanifest
│   ├── sw.js
│   ├── .nojekyll           ← 可选：让 Pages 跳过 Jekyll 处理
│   └── icons/  (120 152 167 180 192 512 1024 + favicon)
└── android/                ← Android 版
    ├── index.html
    ├── manifest.webmanifest
    ├── sw.js
    ├── .nojekyll           ← 同上
    └── icons/  (48 72 96 144 192 512 + maskable 192/512 + favicon)
```

> `.nojekyll` 是空文件，作用只是让 GitHub Pages 跳过 Jekyll 构建、直接按静态文件发布。
> 本项目没有以下划线开头的路径，所以**不上传也不影响使用**；补上可略微加快构建、避免以后加文件踩坑。

---

## 线上地址（已部署）

| 页面 | 地址 |
|---|---|
| 版本选择页 | https://zhalfstar.github.io/liuyao2version/ |
| iPhone / iPad | https://zhalfstar.github.io/liuyao2version/ios/ |
| Android | https://zhalfstar.github.io/liuyao2version/android/ |

仓库：https://github.com/zhalfstar/liuyao2version

---

## 部署方式（换机器 / 重建时参考）

### 方式一：一个仓库放两个版本（当前采用）

1. 在 GitHub 新建一个仓库，例如 `liuyao2version`（Public）。
2. 把 **`github/` 目录里的全部内容**（不是 `github` 文件夹本身）上传到仓库根目录。
3. 仓库 → **Settings → Pages** → Source 选 **Deploy from a branch** → Branch 选 `main` / `(root)` → Save。
4. 等 1～2 分钟，访问：

   - 选择页：`https://你的用户名.github.io/liuyao2version/`
   - iOS 版：`https://你的用户名.github.io/liuyao2version/ios/`
   - Android 版：`https://你的用户名.github.io/liuyao2version/android/`

> 必须走 **https**，`http` 下 Service Worker 不生效，也就不能离线。

### 方式二：两个独立仓库

想让两个版本各自有独立域名，就建两个仓库：

| 仓库 | 放什么 | 访问地址 |
|---|---|---|
| `liuyao-ios` | `ios/` 里的全部内容放到仓库根 | `https://用户名.github.io/liuyao-ios/` |
| `liuyao-android` | `android/` 里的全部内容放到仓库根 | `https://用户名.github.io/liuyao-android/` |

两个仓库都在 Settings → Pages 里选 `main` / `(root)` 发布即可。
因为所有路径都是相对路径（`./`、`icons/...`），放在仓库根或子目录都能正常工作。

---

## 安装到桌面

### iPhone / iPad

1. **必须用 Safari** 打开（Chrome / 微信内置浏览器都不支持添加主屏）。
2. 点底部 **分享** 按钮 → **添加到主屏幕**。
3. 确认名称 → 添加。桌面出现图标，点开是全屏、无地址栏。

### Android

1. 用 **Chrome** 打开。
2. 页面底部会自动弹出「安装到桌面，离线也能摇卦」提示条，点 **安装**。
   或点浏览器右上角 `⋮` → **安装应用 / 添加到主屏幕**。
3. 桌面出现图标，独立窗口运行。

---

## 两版的技术差异

| | iOS 版 | Android 版 |
|---|---|---|
| 图标 | `apple-touch-icon` 120/152/167/180，直角满铺（系统自动裁圆角） | 48/72/96/144/192/512 + **maskable** 192/512（自适应图标，避免被裁掉爻线） |
| manifest | 标准字段，`display: standalone` | 额外 `display_override`、`prefer_related_applications: false` |
| 安装引导 | 检测到 iOS Safari 且未安装时，底部提示「分享 → 添加到主屏幕」 | 监听 `beforeinstallprompt`，弹出原生安装按钮 |
| 安全区 | `viewport-fit=cover` + `env(safe-area-inset-*)` 内边距 | 同样支持 |
| 离线 | Service Worker 缓存整站 | 同左 |

## 更新版本

页面内容更新后，**必须同步改 `sw.js` 里的缓存版本号**，否则老用户会一直看到缓存里的旧页面：

```js
const CACHE = 'liuyao-v1';   // 改成 liuyao-v2、v3……
```

改了版本号，Service Worker 会重新预缓存并在下次打开时替换。

## 数据说明

- 所有案例、断语都存在**本机浏览器的 localStorage**，不上传任何服务器。
- 同一台设备上，**Safari 里存的数据和桌面 App 里的数据是两套**（iOS 的机制），
  换入口时请用案例页的「导出 / 导入」搬一次。
- 清理浏览器数据 / 卸载重装会丢失案例，重要卦例建议定期导出备份。

## 图标

图标为「泰卦」意象：深蓝底 `#1a3a5c`，六条米白爻线，下三阳（实线）、上三阴（断线）。
所有尺寸均由 1024×1024 母版等比缩放生成。
