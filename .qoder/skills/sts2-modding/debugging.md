# 调试与问题解决

## 控制台命令

按 `~` 键（Tab 上方）打开控制台。

### 常用命令

| 命令 | 说明 | 示例 |
|------|------|------|
| `help` | 显示所有命令 | `help` |
| `help <command>` | 查看命令帮助 | `help card` |
| `card <ID>` | 添加卡牌到手牌 | `card SURVIVOR` |
| `relic <ID>` | 获得遗物 | `relic BURNING_BLOOD` |
| `gold <amount>` | 获得金币 | `gold 100` |
| `hp <amount>` | 设置血量 | `hp 50` |
| `energy <amount>` | 设置能量 | `energy 5` |

## 启用日志

### 方法 1: 修改启动脚本

1. 找到游戏根目录的 `launch_xxx.bat` 文件
2. 右键用记事本编辑，添加 `--log` 参数：

```batch
@echo off
"%~dp0SlayTheSpire2.exe" --log --rendering-driver opengl3 %*
```

3. 在根目录创建 `steam_appid.txt`，内容为 `2868840`
4. 双击修改后的 bat 文件运行

### 方法 2: 绕过 Steam

添加 `--force-steam=off` 参数可以不通过 Steam 启动。

## 本地联机测试

创建两个启动脚本：

**主机 (host.bat)**:
```batch
@echo off
"%~dp0SlayTheSpire2.exe" --log --fastmp=host %*
```

**客户端 (client.bat)**:
```batch
@echo off
"%~dp0SlayTheSpire2.exe" --log --fastmp=join -clientId=1001 %*
```

> 如果保存有问题，以管理员模式运行 bat 文件。

## 查看游戏源码

### 方法 1: gdsdecomp（完整反编译）

1. 下载 [gdsdecomp](https://github.com/GDRETools/gdsdecomp)
2. 打开 `gdre_tools.exe` → `RE Tools` → `Recover Project...`
3. 选择 `SlayTheSpire2.pck`，点击 `Extract`
4. 如遇网络问题，在 `Export Settings...` 中关闭 `Download Plugins`
5. 用 Godot 导入 `project.godot`

### 方法 2: ILSpy/dnSpy（仅代码）

打开 `data_sts2_windows_x86_64\sts2.dll` 查看 C# 代码。

- [ILSpy](https://github.com/icsharpcode/ILSpy)
- [dnSpy](https://github.com/dnSpy/dnSpy)

## 常见问题排查

### 模组不加载

| 检查项 | 解决方案 |
|--------|----------|
| JSON 格式错误 | 用 JSON 验证器检查 `{modid}.json` |
| 文件名不一致 | 确保 DLL、PCK、JSON 文件名与 `id` 一致 |
| 缺少依赖 | 检查 `dependencies` 是否正确 |
| DLL 编译错误 | 运行 `dotnet build` 查看错误 |

### 场景/脚本加载失败

| 检查项 | 解决方案 |
|--------|----------|
| 二进制转换 | 禁用"将文本资源转换为二进制" |
| 脚本未注册 | 确保调用 `ScriptManagerBridge.LookupScriptsInAssembly()` |
| 节点命名 | 检查唯一名称 `%` 是否设置正确 |
| 路径错误 | 确保 `res://` 路径正确 |

### 存档问题

模组存档与非模组存档分离：

- **非模组存档**: `AppData\Roaming\SlayTheSpire2\steam\{steamid}\profile1\`
- **模组存档**: `AppData\Roaming\SlayTheSpire2\steam\{steamid}\modded\`

首次使用模组时，需要手动复制存档到 `modded` 目录。

### Spine 动画问题

| 问题 | 解决方案 |
|------|----------|
| 动画不播放 | 检查动画名是否为 `idle_loop`, `attack` 等 |
| 版本不兼容 | 游戏使用 Spine 4.2.43，需要转换 |
| 资源未加载 | 检查 `.atlas`, `.skel`, `.png` 是否都存在 |

## 版本迁移

### 0.99 → 0.100

能量表盘结构变更：

**旧结构**:
```
EnergyCounter (Control)
├── BurstBack (CPUParticles2D) %
├── Layers (Control) %
├── BurstFront (CPUParticles2D) %
└── Label (Label)
```

**新结构**:
```
EnergyCounter (Control)
├── EnergyVfxBack (NParticlesContainer) %
├── Layers (Control) %
├── EnergyVfxFront (NParticlesContainer) %
└── Label (Label)
```

需要反射设置 `_particles` 字段：

```csharp
public partial class MyParticlesContainer : NParticlesContainer
{
    public override void _Ready()
    {
        base._Ready();
        Traverse.Create(this).Field("_particles").SetValue(new Array<GpuParticles2D>());
    }
}
```

## 项目改名

如需重命名模组项目：

1. 修改 `project.godot` 中的 `config/name` 和 `project/assembly_name`
2. 重命名 `{modid}.csproj` 文件
3. 重命名 `{modid}.json` 文件，并更新其中的 `id`
4. 重命名 `{modid}.sln` 文件
5. 重新打包，删除旧的模组文件

## 有用链接

- [BaseLib 仓库](https://github.com/Alchyr/BaseLib-StS2)
- [模组模板](https://github.com/Alchyr/ModTemplate-StS2)
- [Harmony 文档](https://harmony.pardeike.net/articles/basics.html)
- [Godot 文档](https://docs.godotengine.org/zh-cn/4.x/)
- [gdsdecomp](https://github.com/GDRETools/gdsdecomp)
