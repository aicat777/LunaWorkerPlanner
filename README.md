# Luna Worker Planner

这是一个可通过 Git 跨设备安装的 Codex Luna Worker 规划师插件仓库。

插件地址：<https://github.com/aicat777/LunaWorker>

选中插件后，当前对话就是项目规划师：只根据用户提供的内容，以及必要时 `luna_worker` 返回的最终报告制定计划、控制范围和把控整体项目；规划师不直接执行项目变更。

## 在另一台电脑安装

1. 确保该电脑已配置访问此私有仓库的 SSH 密钥。
2. 将仓库作为 Codex marketplace 添加：

   ```text
   codex plugin marketplace add git@github.com:aicat777/LunaWorker.git
   ```

3. 安装插件：

   ```text
   codex plugin add luna-worker-workflow@luna-worker
   ```

4. 将 `agents/luna-worker.toml` 复制到该电脑的 `~/.codex/agents/luna-worker.toml`；如果目标文件已存在，先比较内容再决定是否更新。
5. 重新打开一个 Codex 任务，使插件和自定义子代理配置生效。

也可以先 clone 仓库，再把本地仓库目录作为 marketplace source：

```text
codex plugin marketplace add <仓库本地绝对路径>
codex plugin add luna-worker-workflow@luna-worker
```

## 仓库结构

- `plugins/luna-worker-workflow/`：插件包及 Skill
- `.agents/plugins/marketplace.json`：仓库级 marketplace 清单
- `agents/luna-worker.toml`：全局 `luna_worker` 自定义子代理配置
