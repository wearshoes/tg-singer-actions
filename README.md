# tg-singer-actions
使用[tg-signer](https://github.com/amchii/tg-signer)通过github action实现每日自动化签到

# 使用方法
1. fork本仓库并在你自己的仓库最后续步骤
2. 修改`.github/workflows/main.yml`中的`on.schedule.cron`来指定每日运行时间
3. 仓库中设置以下repository secrets：

    a. SESSION_STRING="your_session_string"

    b. TASK_MAP="your_task_map" (格式见`TASK_MAP_DEMO.json`，任务配置可通过本地tg-signer设置好任务后获取)
4. 测试action

# 故障排查

- Action 固定使用 `tg-signer==0.8.6` 和 `kurigram==2.2.24`，避免不兼容的依赖自动升级导致签到在登录阶段失败。
- 每次签到会加载最近 100 个 Telegram 对话，以建立发送消息所需的 peer 缓存。签到目标必须位于最近 100 个对话内；如果日志提示 peer 不可用，请提高 `.github/workflows/main.yml` 中的 `--num-of-dialogs`。
- 签到命令的原始输出不会写入公开的 Actions 日志。工作流只输出任务序号和脱敏后的错误类别；单个 Telegram 聊天失败也会让 Action 正确标记为失败。
