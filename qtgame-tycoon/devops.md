# qtgame-tycoon · PR 流程

参与 `quanttide/qtgame-tycoon` 开发采用 PR 协作，分三步。

## 1. Fork 到自己的仓库并维护

### 操作

1. 打开 https://github.com/quanttide/qtgame-tycoon ，Fork 到自己的账号；
2. 克隆自己的 fork：`git clone https://github.com/<你的账号>/qtgame-tycoon.git`；
3. 同步上游：`git remote add upstream https://github.com/quanttide/qtgame-tycoon.git`，之后每次开发前 `git fetch upstream && git merge upstream/main`。

### 意外情况

- **克隆错仓库**：`git clone` 了上游地址而不是自己的 fork，后面 `git push` 会因无写权限被拒。用 `git remote -v` 确认 origin 指向 `https://github.com/<你的账号>/qtgame-tycoon.git`。
- **忘了配 upstream**：只在自己 fork 上闷头开发，代码越来越旧，PR 会夹带无关 diff、合入时冲突。第一次就配好 upstream，每次开工前先同步。
- **在 fork 的 main 上直接开发**：fork 的 main 与上游同名，直接提交后发 PR，会把整条提交历史都带进 PR。先 `git checkout -b <分支名>` 建分支再写代码。

## 2. 提交者 PR，评审者复核

### 操作

1. 提交者在分支上开发，本地提交触发 pre-commit 质量门（ESLint + TypeScript 类型检查），通过后把分支推到自己的 fork，向上游 `main` 发起 PR；
2. 评审者复核三件事：锁定决策是否被改动（见仓库 `AGENTS.md`）、`src/app` 与 `src/engine` 分层边界是否守住、检查是否通过；
3. 不通过则留言退回，提交者修改后推同一分支，PR 自动更新，直到批准。

### 意外情况

- **第一次提交被拒绝**：`.husky/pre-commit` 跑 `npx eslint . && npx tsc -b`，没过就提交不了。按报错修复再提交，不要用 `--no-verify` 绕过。
- **分层边界被 ESLint 拦**：`src/app/` 直接 import `mutate/reducer/events` 会报 `no-restricted-imports`。这是架构约束不是风格问题，写操作改走 dispatch → reducer。
- **PR 显示冲突**：上游 `main` 前进了。`git fetch upstream && git merge upstream/main`，解决冲突再推。
- **评审退回**：按意见修改，推同一个分支即可，PR 会自动更新，不用关掉重开。

## 3. 通过以后自动触发构建

PR 合入 `main` 后 GitHub Actions 自动构建发布，主站（`tycoon.game.quanttide.com`）与 GitHub Pages 各一条 workflow，均绿即发布成功。CI 已配置妥当，一般不会出问题；若构建变红或线上没有更新，联系维护者处理。
