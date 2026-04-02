## 1. 准备工作

- [x] 1.1 确定 skill 文件存放位置（.qoder/skills/ 目录）
- [x] 1.2 创建 skill 目录结构

## 2. 编写 Skill 核心内容

- [x] 2.1 编写 skill 元信息（名称、描述、触发条件）
- [x] 2.2 编写环境配置章节（Godot 4.5.1 Mono、.NET SDK 9+、VS Code 插件）
- [x] 2.3 编写项目结构章节（.csproj 配置、Entry.cs 模板、JSON 配置）
- [x] 2.4 编写模组文件规范章节（DLL、PCK、JSON 三件套说明）

## 3. 编写 BaseLib 接口指南

- [x] 3.1 编写添加卡牌代码模板（CustomCardModel 继承示例）
- [x] 3.2 编写添加遗物代码模板（CustomRelicModel 继承示例）
- [x] 3.3 编写添加药水代码模板（CustomPotionModel 继承示例）
- [x] 3.4 编写添加能力代码模板（CustomPowerModel 继承示例）
- [x] 3.5 编写添加人物指南（卡池配置、PlaceholderCharacterModel 继承）

## 4. 编写场景与资源规范

- [x] 4.1 编写人物模型场景节点结构（Visuals、Bounds、IntentPos、CenterPos）
- [x] 4.2 编写能量表盘场景结构（EnergyVfxBack、Layers、EnergyVfxFront、Label）
- [x] 4.3 编写 Spine 动画导入说明（4.2.43 版本要求、动画名称规范）

## 5. 编写本地化与描述系统

- [x] 5.1 编写 BBCode 标签速查表（原生标签 + 游戏自定义标签）
- [x] 5.2 编写占位变量说明（Damage、Block、Energy 等）
- [x] 5.3 编写 formatter 用法（diff()、energyIcons()、starIcons() 等）
- [x] 5.4 编写先古对话 JSON 格式规范（ID 格式、对话序号机制）

## 6. 编写 Harmony 补丁模式

- [x] 6.1 编写 Harmony 初始化模板
- [x] 6.2 编写 Prefix/Postfix 补丁示例
- [x] 6.3 编写卡图替换补丁代码
- [x] 6.4 编写皮肤替换补丁代码

## 7. 编写调试与问题解决

- [x] 7.1 编写控制台命令说明（~ 键打开、help 命令）
- [x] 7.2 编写日志启用方法（--log 参数、steam_appid.txt）
- [x] 7.3 编写本地联机测试配置（--fastmp=host/join）
- [x] 7.4 编写常见问题排查清单

## 8. 编写版本兼容性

- [x] 8.1 标注适用游戏版本（public-beta）
- [x] 8.2 编写 0.99 至 0.100 迁移指南摘要
- [x] 8.3 添加版本更新提醒说明

## 9. 验证与发布

- [x] 9.1 检查 skill 文件格式是否符合 Qoder 规范
- [x] 9.2 测试 skill 在模组开发场景中的效果
- [x] 9.3 将 skill 文件添加到项目中
