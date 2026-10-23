# Waggle_NEXT

**Waggle_NEXT** 是 [anywhere-labs/dsh-desktop](https://github.com/anywhere-labs/dsh-desktop) 的自用衍生版（MIT）。

> 名字取自蜜蜂的「摇摆舞」（waggle dance）：用舞蹈把方向与距离告诉同伴——正如多 Agent 之间靠任务总线传递指令。

## 与上游的差异（仅 5 处，全部是配置/命名）

| # | 文件 | 改动 | 目的 |
|---|---|---|---|
| C1 | `dsh-desktop-next/package.json` → `build.appId` | `ai.deepseek.dsh.desktop.next` → `ai.hive.wagglenext` | 让系统视为**独立应用**，可与上游版本**同时运行** |
| C2 | 同上 → `build.productName` | `DSH NEXT` → `Waggle_NEXT` | 窗口标题 / 任务栏 / 安装目录名 |
| C3 | 同上 → `win.artifactName` / `nsis.artifactName` / `nsis.shortcutName` | `DSH-NEXT-*` → `Waggle-NEXT-*` | 产物与快捷方式命名 |
| C4 | `dsh-desktop-next/build/app-icon.ico` | 替换为自用图标（多尺寸 16–256） | 图标 |
| C5 | `dsh-desktop-next/scripts/verify-fuses.ts` | `'win-unpacked/DSH NEXT.exe'` → `'win-unpacked/Waggle_NEXT.exe'` | 该脚本硬编码旧 exe 名，改名后会误报 |

**没有改动**：包名、单实例锁、app.asar 内容、插件逻辑。上游更新时最多 10 分钟合并。

## 构建

```bash
corepack enable                       # 用仓库锁定的 yarn 4.18
git submodule update --init --depth 1 # 内核子模块 deepseek-harness
corepack yarn install --mode=skip-build
node dsh-desktop-next/node_modules/electron/install.js   # 上游 enableScripts=false，需手工装 Electron 二进制
corepack yarn build                   # 必须全仓构建（否则 market 插件缺产物）
corepack yarn workspace dsh-desktop-next package:dir     # 出 dist/win-unpacked
# 或完整安装包：corepack yarn workspace dsh-desktop-next dist:win
```

## 使用与授权

- **许可**：继承上游 MIT（见 [LICENSE](./LICENSE)）。
- **只读仓库**：本仓库为**公开只读**——欢迎查看、Fork、提 Issue；**不接受外部直接提交**（主分支仅维护者推送）。
- **用途**：内部自用工程底座，不提供官方支持，不对第三方使用后果负责。
- 与上游同步方式：跟随上游 `master`，重新套用上述 5 处补丁。
