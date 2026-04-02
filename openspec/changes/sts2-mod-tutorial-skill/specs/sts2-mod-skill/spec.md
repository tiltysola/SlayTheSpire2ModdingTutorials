## ADDED Requirements

### Requirement: Skill 包含完整的环境配置指南
Skill 文件 SHALL 包含 STS2 模组开发所需的完整环境配置信息，包括 Godot 版本、.NET SDK 版本、VS Code 插件配置等。

#### Scenario: 用户首次配置开发环境
- **WHEN** 用户请求配置 STS2 模组开发环境
- **THEN** AI 提供 Godot 4.5.1 Mono 安装指南、.NET SDK 9+ 下载链接、VS Code 插件列表

#### Scenario: 用户询问项目创建步骤
- **WHEN** 用户需要创建新的模组项目
- **THEN** AI 指导创建 Godot 项目、生成 C# 解决方案、配置 .csproj 文件引用游戏 DLL

### Requirement: Skill 包含模组文件结构规范
Skill 文件 SHALL 明确定义模组的三件套文件结构 (DLL、PCK、JSON) 及其作用。

#### Scenario: 用户询问模组组成
- **WHEN** 用户询问模组包含哪些文件
- **THEN** AI 说明 DLL (代码)、PCK (资源)、JSON (配置) 的作用和创建方法

#### Scenario: 用户需要导出模组
- **WHEN** 用户完成开发需要导出模组
- **THEN** AI 提供 dotnet build 编译 DLL 和 Godot 导出 PCK 的具体步骤

### Requirement: Skill 包含 BaseLib 接口使用指南
Skill 文件 SHALL 提供 BaseLib 添加新内容（卡牌、遗物、药水、能力、人物等）的完整接口说明和代码模板。

#### Scenario: 用户添加新卡牌
- **WHEN** 用户请求添加自定义卡牌
- **THEN** AI 提供继承 CustomCardModel 的完整代码模板，包含费用、伤害、描述等属性配置

#### Scenario: 用户添加新遗物
- **WHEN** 用户请求添加自定义遗物
- **THEN** AI 提供继承 CustomRelicModel 的代码模板和效果触发实现

#### Scenario: 用户添加新人物
- **WHEN** 用户请求添加自定义角色
- **THEN** AI 提供 PlaceholderCharacterModel 继承模板、卡池配置、场景文件结构说明

### Requirement: Skill 包含 Harmony 补丁模式
Skill 文件 SHALL 提供使用 Harmony 库修改游戏代码的标准模式和示例。

#### Scenario: 用户需要修改游戏行为
- **WHEN** 用户需要修改原版游戏的某个功能
- **THEN** AI 提供 HarmonyPatch 装饰器的 Prefix/Postfix/Transpiler 用法示例

#### Scenario: 用户替换卡图或皮肤
- **WHEN** 用户需要替换原版卡牌图片或角色皮肤
- **THEN** AI 提供 patch CardModel.PortraitPath 或 NCreature._Ready 的代码模板

### Requirement: Skill 包含场景节点结构规范
Skill 文件 SHALL 说明自定义场景（人物模型、能量表盘等）所需的节点结构和命名规则。

#### Scenario: 用户创建人物模型场景
- **WHEN** 用户需要创建自定义人物战斗模型
- **THEN** AI 说明 Node2D 根节点下需要 Visuals、Bounds、IntentPos、CenterPos 子节点，且需设置唯一名称 %

#### Scenario: 用户创建能量表盘场景
- **WHEN** 用户需要创建自定义能量表盘
- **THEN** AI 说明 Control 根节点下需要 EnergyVfxBack、Layers、EnergyVfxFront、Label 节点结构

### Requirement: Skill 包含本地化文件格式
Skill 文件 SHALL 提供本地化 JSON 文件的格式规范和 BBCode 标签说明。

#### Scenario: 用户配置卡牌描述
- **WHEN** 用户需要编写卡牌或遗物的描述文本
- **THEN** AI 提供占位变量（{Damage}、{Block}等）和 formatter（diff()、energyIcons()等）的用法

#### Scenario: 用户配置人物对话
- **WHEN** 用户需要配置先古之民对话
- **THEN** AI 说明 ancients.json 的 ID 格式规则和对话序号机制

### Requirement: Skill 包含调试和问题解决方法
Skill 文件 SHALL 提供常见问题的调试方法和解决方案。

#### Scenario: 用户遇到模组加载失败
- **WHEN** 用户的模组无法正常加载
- **THEN** AI 指导检查 JSON 配置、DLL 依赖、文件命名一致性

#### Scenario: 用户需要查看日志
- **WHEN** 用户需要调试模组运行时问题
- **THEN** AI 说明如何修改 launch_xxx.bat 添加 --log 参数启用日志输出

### Requirement: Skill 包含版本兼容性说明
Skill 文件 SHALL 标注适用的游戏版本和 BaseLib 版本，并提供版本迁移指南。

#### Scenario: 用户查询版本兼容性
- **WHEN** 用户询问 skill 适用的游戏版本
- **THEN** AI 说明当前适用于 public-beta 分支，并提醒查阅最新教程了解版本变更

#### Scenario: 用户需要版本迁移
- **WHEN** 游戏版本更新导致模组不兼容
- **THEN** AI 提供迁移指南，如 0.99 至 0.100 的能量表盘结构变更
