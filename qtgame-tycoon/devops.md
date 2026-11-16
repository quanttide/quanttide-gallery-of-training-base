# qtgame-tycoon · PR 流程

参与 `quanttide/qtgame-tycoon` 开发采用 PR 协作，分三步。

## 1. Fork 到自己的仓库并维护

1. 打开 https://github.com/quanttide/qtgame-tycoon ，Fork 到自己的账号；
2. 克隆自己的 fork 开发：`git clone https://github.com/<你的账号>/qtgame-tycoon.git`；
3. 维护 fork：开发前先同步上游最新代码（`git remote add upstream https://github.com/quanttide/qtgame-tycoon.git`，之后 `git fetch upstream && git merge upstream/main`），避免在过时代码上开发。

## 2. 提交者 PR，评审者复核

1. 提交者在 fork 上建分支开发；本地提交会触发 pre-commit 质量门（ESLint + TypeScript 类型检查），通过后推送分支，向 `quanttide/qtgame-tycoon` 的 `main` 发起 PR；
2. 评审者复核：玩法与页面的锁定决策是否被改动（见仓库 `AGENTS.md`）、`src/app` 与 `src/engine` 的分层边界是否守住、检查是否通过；通过则批准，不通过则留言要求修改；
3. 提交者按评审意见修改，推送同一分支，PR 自动更新，直到批准。

## 3. 通过以后自动触发构建

PR 合入 `main` 后自动触发 GitHub Actions：`npm ci` + `npm run build`（`tsc -b` 类型检查 + Vite 构建），主站链路将产物上传 OSS 桶并刷新 CDN（`tycoon.game.quanttide.com`）。构建失败即发布失败，需要修复后重新推送。
