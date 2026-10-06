# 冰箱管家 · Pantry Steward

把冰箱里快到期的食材，变成今晚可以执行的一顿饭。

冰箱管家是基于 OctoSense 官方宿主运行的独立 OctoScript 应用，参赛方向为 **OctoSense + AppCard 场景应用**。

当前应用版本：**0.6.1**  
已验证平台：**Windows**  
Android 及其他平台：**尚未验证**

## 项目简介

买了食材后，我们常常忘记剩余数量和到期时间；决定吃什么时，又需要重新整理库存、偏好和饮食目标。

冰箱管家围绕已确认的库存完成用餐规划：

```text
建立饮食档案 → 确认周期方案 → 确认食材入库
                                  ↓
输入本餐目标 → 生成菜单候选 → 查看做法并接受方案
                                  ↓
确认实际食用量 → 扣减库存并记录摄入 → 继续规划
```

生成菜单、接受方案和确认食用是三个独立操作。只有用户最终确认实际用量后，应用才扣减库存、累计摄入。

## 演示与截图

[查看演示视频](video/演示视频.mp4)

![冰箱管家首页](bundle/screenshots/01-main.png)

[菜单方案](bundle/screenshots/02-plan.png) · [用量确认](bundle/screenshots/03-confirm.png) · [库存更新](bundle/screenshots/04-updated.png)

以上截图来自此前的官方 Windows 桌面运行记录，不代表新增周期功能的完整截图，也不是手机真机截图。

## 已实现功能

- **饮食档案：** 保存基础资料、饮食偏好与忌口。
- **周期方案：** 选择并确认 7、21 或 30 天周期，在档案中查看记录和进度。
- **库存管理：** 按批次录入食材、克数和剩余天数，修改与删除均需确认。
- **菜单规划：** 根据库存和目标生成候选，展示推荐原因、预计时长、用量与做法。
- **在线 AI：** 通过官方宿主的 `model.complete` 服务请求菜单，由宿主管理提供方与密钥。
- **本地规则：** 不调用模型，按库存和临期顺序安排简单搭配。
- **食用确认：** 核对实际克数，校验超量、方案有效性与重复扣减。
- **方案删除：** 经确认删除菜谱，不回滚库存或删除已记录的摄入。
- **本地保存：** 保存档案、库存、周期、方案和执行记录。
- **周期续接：** 存档旧周期，保留库存，再选择下一周期。

### 手动与自动生成

默认采用手动生成。

库存变化、临期检查和确认食用默认只更新数据或显示提醒，不自动调用模型。用户明确点击生成、备选或重试后，才开始规划。

用户可在设置中主动开启自动生成。即使开启，接受菜单和确认扣减仍需用户操作。已经发送的模型请求可能产生费用，停止等待不保证撤销请求或计费。

## 目录与固定版本

本仓库的 `main` 分支是应用目录：

```text
bundle/              应用包、图标、背景和截图
tools/               启动与检查工具
run.cmd              Windows 启动入口
CYCLE-PLANS.md       周期方案说明
VALIDATION.md        验证记录
PRIVACY.md           隐私说明
REVIEW-ANSWERS.md    审核问题回答
video/               演示视频
```

这里不包含完整的 OctoSense 宿主工程。

当前启动器依赖宿主工程中的 `Cargo.toml`、运行时锁文件和框架依赖。**不能只克隆 main 后，在任意目录直接运行 `run.cmd`。**

比赛固定应用版本为：

- Tag：[`v0.6.1`](https://github.com/woshuoduijiushidui/OctoSense-AppCard/tree/v0.6.1)
- Commit：`3fb6be2273dce840b3e660e92c61ff066c632f38`
- 此标签中的应用包路径：`apps/pantry-steward/bundle`

`v0.6.1` 包含完整宿主工程；main 中的最新说明和视频是补充材料，不属于该标签。

## Windows 源码运行

### 环境要求

需要：

- Git
- Python 3.11 或更新版本
- Rust stable
- Windows C++ 编译工具与 Windows SDK
- 首次准备依赖所需的网络与磁盘空间

这是源码项目，不是预编译安装包。首次准备和编译可能需要较长时间。

### 获取完整固定版本

在终端执行：

```cmd
git clone --branch v0.6.1 --single-branch https://github.com/woshuoduijiushidui/OctoSense-AppCard.git OctoSense-Pantry-v0.6.1
cd OctoSense-Pantry-v0.6.1
python -X utf8 tools/setup.py
```

如果本机已有 Makepad 等框架依赖，请按 [官方环境说明](https://github.com/OctoSense-org/OctoSense) 配置依赖目录复用，避免重复下载。

上述新机器完整下载流程尚未重新进行冷启动验证；已有 Windows 环境的运行记录见 [VALIDATION.md](VALIDATION.md)。

### 首次启动

在完整工程根目录执行：

```cmd
apps\pantry-steward\run.cmd --prepare-local-test
```

此参数表示同意生成本机测试密钥，用于创建本地签名目录并通过官方安装检查。测试密钥保存在仓库之外，不是正式发布者密钥，也不会将应用提交到远程 App Hub。

### 后续启动

仍在完整工程根目录执行：

```cmd
apps\pantry-steward\run.cmd
```

启动失败时请保留终端错误信息。不要直接删除 `.local-state`，它包含本机应用数据；删除前应备份。

## 使用方法

1. 填写基础资料、偏好与忌口。
2. 查看周期候选，选择并确认一个周期。
3. 录入食材、克数和剩余天数，预览后确认入库。
4. 在首页输入本餐目标，点击生成。
5. 查看候选的做法与用量，确认接受方案。
6. 实际吃完后核对真实用量，再确认扣减。
7. 在冰箱与档案页面查看库存变化和周期记录。

具体周期规则见 [CYCLE-PLANS.md](CYCLE-PLANS.md)。

## 在线 AI 配置

本版本使用 **OctoSense 官方宿主的 AI providers 设置**。

应用不读取 `ai.env`，不要求用户在应用内填写 API Key，也不在脚本中直接访问模型提供方。请在启动器打开的测试桌面中配置宿主的模型提供方。

```text
应用提交目标、档案和库存
          ↓
官方 model.complete 服务
          ↓
宿主管理的模型提供方
          ↓
应用校验结果，展示待确认菜单
```

`model.budget` 查询成功不代表模型已配置或生成已成功。应以实际请求结果和菜单来源标记判断。

用户曾报告在线模型生成成功；自动回归记录主要使用本地规则，不代表已经完成 0.6.1 的完整在线模型测试。

## 验证情况与边界

应用包检查结果：

```text
pantry-steward 0.6.1 — PASSED
[warning] publisher-signature: unsigned: accountability rests on the hub alone
grants: capabilities {"model", "storage"}, hosts {}, storage 16777216 bytes, agent none
```

启动器的 7 项测试通过。周期功能的隔离 Windows 测试及历史界面测试见 [VALIDATION.md](VALIDATION.md)。

检查通过不等于新机器构建通过、所有功能验证完成、比赛资格确认或正式商店审核通过。

当前限制：

- 麦克风图标仅显示语音识别不可用提示，不录音或上传音频。
- 未实现照片识别、小票 OCR 或买菜下单。
- 宿主关闭后不执行后台临期检查。
- 未验证 Android、iOS、macOS 和 Linux。
- 能量、营养与周期目标为原型估算，不是医疗或专业营养建议。
- 食材到期信息由用户填写，应用不能保证食品安全。
- 未知食材参与数值核算前，需要补齐包装标签成分。

## 数据与隐私

库存、档案、方案和执行记录保存在应用自己的存储目录中。

在线规划会向宿主管理的模型提供方发送本餐目标、饮食档案、库存和成分参考、周期及余额等规划所需信息。提供方的数据处理与费用以其政策和用户账户设置为准。

应用不收集密码或 API Key，不读取其他应用数据；未接入广告与分析统计。

当前 [PRIVACY.md](PRIVACY.md) 仍为待作者确认的说明草稿，正式发布前需完成确认。

## App Hub 提交

已创建提交请求：

[Submit pantry-steward 0.6.1 · Issue #99](https://github.com/OctoSense-org/OctoSense-App-Hub/issues/99)

该请求指定 `v0.6.1`、完整提交号与 `apps/pantry-steward/bundle` 路径，并提供检查输出和审核问题回答。

**已提交审核不代表已上架。** 是否通过及是否进入正式目录，以 App Hub 维护者反馈为准。

比赛提交无需等待商店上架；仓库、固定版本、演示视频及成员资料应按主办方或老师最新通知提交。

## 作者与来源

作者：leoniaodo、zix、power胖丸、Roooy。

基础宿主：[OctoSense](https://github.com/OctoSense-org/OctoSense)  
开发参考：[OctoScript App Design Flow](https://github.com/OctoSense-org/OctoScript-App-Design-Flow)  
发布规范：[OctoSense App Hub](https://github.com/OctoSense-org/OctoSense-App-Hub)

## 许可证

项目采用 Apache License 2.0。

完整固定版本中的 [LICENSE](https://github.com/woshuoduijiushidui/OctoSense-AppCard/blob/v0.6.1/LICENSE) 与 [NOTICE](https://github.com/woshuoduijiushidui/OctoSense-AppCard/blob/v0.6.1/NOTICE) 可供查阅。分发源码时，应保留适用的许可证和第三方来源声明。