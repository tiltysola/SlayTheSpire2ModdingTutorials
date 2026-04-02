## Why

当前项目已有完整的《杀戮尖塔2》模组制作教程文档，涵盖环境配置、代码修改、BaseLib接口、新人物添加、视觉资源制作等内容。将这些知识整合为一个 Qoder Skill，可以让 AI 助手在帮助用户制作 STS2 模组时，自动应用最佳实践和正确的代码模式，显著提升模组开发效率。

## What Changes

- **新增 STS2 模组制作 Skill**：创建一个 markdown 格式的 skill 文件，包含：
  - 环境配置要求（Godot 4.5.1 Mono、.NET SDK 9+、Harmony库）
  - 模组文件结构规范（DLL、PCK、JSON三件套）
  - BaseLib接口使用指南（卡牌、遗物、药水、能力、人物等）
  - 代码模式和最佳实践（Harmony补丁、ModInitializer、场景节点结构）
  - 本地化文件格式和BBCode标签
  - 常见问题和调试方法

## Capabilities

### New Capabilities

- `sts2-mod-skill`: 杀戮尖塔2模组制作的完整知识库，包含项目配置、代码模式、资源规范、最佳实践等

### Modified Capabilities


## Impact

- 创建新的 skill 文件，供 Qoder 在处理 STS2 模组开发相关任务时参考
- 用户在进行模组开发时，AI 将能够自动应用正确的代码模式和项目结构
- 无需修改现有教程文档，skill 将作为独立的知识抽取
