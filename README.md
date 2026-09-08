# Luna Worker Planner

这是一个公开、可供所有人使用的 Codex Luna Worker 规划师插件仓库。

插件地址：<https://github.com/aicat777/LunaWorker>

选中插件后，当前对话就是项目规划师：只根据用户提供的内容，以及必要时 `luna_worker` 返回的最终报告制定计划、控制范围和把控整体项目；规划师不直接执行项目变更。

## 在其他设备安装

公开仓库不要求 SSH 密钥。推荐先用 HTTPS clone，这样可以同时取得全局 `luna_worker` 配置：

```text
git clone https://github.com/aicat777/LunaWorker.git <本地目录>/LunaWorker
```

然后将仓库作为 Codex marketplace 添加：

```text
codex plugin marketplace add <本地目录>/LunaWorker
```

安装插件：

```text
codex plugin add luna-worker-workflow@luna-worker
```

将 `agents/luna-worker.toml` 复制到该设备的 `~/.codex/agents/luna-worker.toml`；如果目标文件已存在，先比较内容再决定是否更新。

也可以直接添加公开 Git marketplace：

```text
codex plugin marketplace add https://github.com/aicat777/LunaWorker.git
codex plugin add luna-worker-workflow@luna-worker
```

最后重新打开一个 Codex 任务，使插件和自定义子代理配置生效。

SSH 仍可作为可选方式使用：`git@github.com:aicat777/LunaWorker.git`。

## 仓库结构

- `plugins/luna-worker-workflow/`：插件包及 Skill
- `.agents/plugins/marketplace.json`：仓库级 marketplace 清单
- `agents/luna-worker.toml`：全局 `luna_worker` 自定义子代理配置
