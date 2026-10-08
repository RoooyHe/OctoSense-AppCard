# 0.6.2 提交准备 / Publication checkpoint

这是本地候选的清单，不是已发布证明或合格评分。

- 版本：pantry-steward 0.6.2；包路径：bundle/。
- 旧提交：[0.6.1 / #99](https://github.com/OctoSense-org/OctoSense-App-Hub/issues/99)，其标签及审核记录不变。
- 发布者姓名、Windows-only 声明及向宿主模型提供方发送规划输入的隐私说明：用户已确认。
- 背景图 sunlit-pantry-bg.png：作者于 2026-10-08 确认是本人原创，已记录 NOTICE；不声称独立权属鉴定。
- 支持：https://github.com/woshuoduijiushidui/OctoSense-AppCard/issues
- 隐私：https://github.com/woshuoduijiushidui/OctoSense-AppCard/blob/main/PRIVACY.md
- 源码检查、单元测试及尚未验证项见 [VALIDATION.md](VALIDATION.md)。

## 发布前仍须本人确认

1. 审阅 .github/workflows/publish-app.yml；它只在推送 v* 标签时运行，发布作业获得 contents、id-token、attestations 写权限。没有开发者签名 secret。
2. 允许提交、推送并创建新的 v0.6.2 标签。当前任务只获准本地修复，尚未做这些外部动作。
3. 老师是否接受截止后的改进版本；源码修改不自动改变比赛截止规则。

## 授权后按最新版规则完成

依照[官方提交说明](https://github.com/OctoSense-org/OctoSense-App-Hub/blob/main/docs/SUBMITTING.md)：

- 用当前工具链完成源包检查、原生测试和 hub scan，所有问题逐项回答。
- 发布经审阅的源码与工作流，使用新的精确版本标签，不移动 v0.6.1。
- GitHub 工作流成功后下载 app.bundle.pack.json、规范 manifest 和 release-receipt.json；核对 SHA256，并用 publisher-unpack / publisher-verify 校验，不能对封存包重新 stamp。
- 为 **0.6.2 创建新 Issue**，关联 #99，填写实际标签、完整 SHA、bundle/ 路径、工作流 URL、Release 与 pack URL、校验输出、截图、隐私与支持链接。
- 未运行的项目标记 pending；不要用旧版 PASSED 冒充新发布证明。

GitHub 工作流成功、比赛评分、维护者审批、目录发布及兼容宿主安装是不同环节。目前不保证任何审批结果。
