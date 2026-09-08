# Luna Worker Planner

## 安装

添加 marketplace：

```text
codex plugin marketplace add https://github.com/aicat777/LunaWorker.git
```

安装插件：

```text
codex plugin add luna-worker-workflow@luna-worker
```

将仓库中的 `agents/luna-worker.toml` 放入当前设备的 `~/.codex/agents/luna-worker.toml`，然后重新打开一个 Codex 对话。

在对话中选择 `@Luna Worker Planner`。当项目上下文不足时，规划师会自动调用 `luna_worker` 做一次只读项目侦察，等待最终报告后再制定计划；规划师本身不执行项目变更。

## 更新

```text
codex plugin marketplace upgrade luna-worker
codex plugin add luna-worker-workflow@luna-worker
```
