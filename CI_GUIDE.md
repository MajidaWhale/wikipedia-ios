# 使用 GitHub Actions 自动编译 Wikipedia iOS IPA

## 前置条件

1. **一个 GitHub 账号**
2. 把这个仓库 fork 到你自己的 GitHub 账号

## 步骤

### 1. Fork 代码到你的 GitHub

在你的浏览器中操作，或者我帮你创建好仓库。

### 2. 在 GitHub 上触发编译

- 进入你 fork 的仓库
- 点击 **Actions** 标签
- 选择 **Build IPA** workflow
- 点击 **Run workflow**
- 选择 Scheme（Wikipedia / Staging / Experimental）
- 选择 Build Type（debug / release）
- 点击 **Run workflow**

### 3. 下载 IPA

编译完成后（约 15-30 分钟）：

- **debug 模式** → 产出 `.app`（可在模拟器运行）
- **release 模式** → 产出 `.ipa`（可在真机安装）

> ⚠️ **重要**：release 模式产出的 IPA 是 **未签名** 的。要在真机上安装，你需要：
> - 有 Apple Developer 账号（$99/年）
> - 使用自己的证书签名后才能安装到 iPhone

### 4. 本地签名 IPA（在自己的 Mac 上）

下载 IPA 后，在你的 Mac 上运行：

```bash
# 解压 IPA
unzip Wikipedia.ipa -d Payload

# 用你的证书重签名
codesign -f -s "iPhone Developer: YOUR_NAME (YOUR_TEAM_ID)" \
  --entitlements Wikipedia.entitlements \
  Payload/Wikipedia.app

# 重新打包
zip -r Wikipedia-signed.ipa Payload/
```

## Workflow 文件

[`build-ipa.yml`](minis://workspace/wikipedia-ios/.github/workflows/build-ipa.yml)
