# 月历生成器 · 无服务器版本更新（GitHub 方案）

利用 GitHub 免费托管版本信息 + APK，App 内点击"检查更新"即可发现新版本并直接下载安装。**无需任何服务器，零成本。**

## 原理

```
App 检查更新
   │  请求 update.json（GitHub Pages）
   ▼
版本号比较：GitHub 上的 versionCode > 本机 versionCode？
   ├─ 否 → 提示"已是最新版本"
   └─ 是 → 弹出更新对话框（显示新版本号 + 更新说明）
              └─ 点"立即下载" → App 内直接下载 APK
                                  → 下载完成自动调起系统安装界面
```

## 三步搭建（约 5 分钟）

### 第一步：创建 GitHub 仓库

1. 打开 https://github.com/new（需登录 GitHub，没有就先注册一个免费账号）
2. 仓库名建议填：`calendar-app-update`（可自定义）
3. 选 **Public**（公开），其他默认，点 **Create repository**

### 第二步：上传 update.json

1. 点 **Add file → Upload files**，上传本目录下的 `update.json`
2. 先编辑 `update.json`，把 `apkUrl` 里的"Silas2027/calendar-app-update"换成你自己的：
   ```json
   {
     "versionCode": 6,
     "versionName": "1.5",
     "apkUrl": "https://github.com/Silas2027/calendar-app-update/releases/download/v1.5.0/月历生成器_v1.5.apk",
     "changelog": "1. 新增：App 内直接下载并安装新版本"
   }
   ```
3. 点 **Commit changes** 保存

### 第三步：开启 GitHub Pages（让 App 能访问到 update.json）

1. 仓库页面 → **Settings** → 左侧 **Pages**
2. Branch 选 `main`，文件夹 `/ (root)` → 点 **Save**
3. 等 1-2 分钟后，你的更新地址就是：
   ```
   https://Silas2027.github.io/calendar-app-update/update.json
   ```
   （例如 `https://zhangsan.github.io/calendar-app-update/update.json`）

### 第四步：上传 APK 到 Releases（放新版本安装包）

1. 仓库页面 → **Releases**（右侧）→ **Create a new release**
2. Tag 填 `v1.5.0`，标题填 `v1.5.0`
3. 把 `月历生成器_v1.5.apk` 拖进附件框上传
4. 点 **Publish release**
5. 确认 APK 下载地址是：
   ```
   https://github.com/Silas2027/calendar-app-update/releases/download/v1.5.0/月历生成器_v1.5.apk
   ```
   与 `update.json` 里 apkUrl 完全一致

### 第五步：App 里填入更新地址

打开 App → 功能设置 → 版本更新 → 更新地址输入框 → 填入你的 GitHub Pages 地址：
```
https://Silas2027.github.io/calendar-app-update/update.json
```
点"保存"即可，之后点"检查更新"就能检测到 GitHub 上的新版本。

## 以后发布新版本怎么做

每次发布新版本只需两步（都在 GitHub 网页上完成，无需重新编译 App 逻辑）：

1. **传新 APK**：Releases → New release → Tag 改为 `v1.6.0` 等 → 上传新 APK → Publish
2. **改 update.json**：在仓库里编辑 update.json，把 `versionCode` 加 1、`versionName` 改新版本号、`apkUrl` 改成新 Tag 的下载地址、更新 changelog → Commit

> versionCode 必须**比当前安装版本大**才会提示更新（例如当前装的是 6，新版本就要写 7）。

## 常见问题

| 问题 | 解决 |
| :--- | :--- |
| 点检查更新提示"检查服务暂不可用" | 检查 update.json 地址是否填对；确认 GitHub Pages 已开启（等 1-2 分钟再试） |
| 提示"已是最新版本"但明明发布了新版 | 确认 update.json 里 versionCode 大于本机版本号 |
| 下载失败 | 确认 apkUrl 与 Releases 实际地址一致（大小写、Tag 名） |
| 点立即下载后提示"无法启动安装" | 系统设置 → 应用 → 月历 → 允许安装未知应用 |
