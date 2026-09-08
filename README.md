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

将仓库中的 `agents/luna-worker.toml` 放入当前设备的 `~/.codex/agents/luna-worker.toml`。安装完成后重新打开一个 Codex 对话，并选择 `@Luna Worker Planner`。

## 更新

```text
codex plugin marketplace upgrade luna-worker
codex plugin add luna-worker-workflow@luna-worker
```
