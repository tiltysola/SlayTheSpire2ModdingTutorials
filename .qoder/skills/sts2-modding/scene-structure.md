# 场景节点结构规范

## 人物战斗模型场景

路径示例: `res://mymod/scenes/my_character.tscn`

```
MyCharacter (Node2D)
├── Visuals (Node2D) %          ← 唯一名称，放置角色视觉元素
│   └── SpineBody 或 Sprite2D
├── Bounds (Control) %          ← 唯一名称，定义碰撞边界
├── IntentPos (Marker2D) %      ← 唯一名称，意图图标位置
└── CenterPos (Marker2D) %      ← 唯一名称，角色中心点
```

**关键要点**:
- 使用 `%` 设置节点为唯一名称（在 Godot 中右键节点 → Access as Unique Name）
- 支持 Spine 动画、Sprite2D 静态图或 AnimatedSprite2D

**必需的 Spine 动画名**:
| 动画名 | 用途 |
|--------|------|
| `idle_loop` | 待机循环 |
| `attack` | 攻击动作 |
| `cast` | 能力卡动作 |
| `hurt` | 受伤反应 |
| `die` | 死亡动画 |

## 能量表盘场景

路径示例: `res://mymod/scenes/my_energy_counter.tscn`

```
MyEnergyCounter (Control)
├── EnergyVfxBack (NParticlesContainer) %   ← 背景粒子效果
├── Layers (Control) %                       ← 图层容器
│   ├── Layer1 (TextureRect)                 ← 底层图片
│   └── RotationLayers (Control) %           ← 旋转图层（可选）
├── EnergyVfxFront (NParticlesContainer) %  ← 前景粒子效果
└── Label (Label)                            ← 能量数字显示
```

**NParticlesContainer 反射设置**（版本 0.100+）:

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

## 人物背景场景

用于角色选择界面的背景。

```
MyCharacterBackground (Control)
└── ... 自定义背景元素
```

## Spine 动画导入

### 版本要求

游戏使用 **Spine 4.2.43** 版本，其他版本导出的文件需要转换。

### 导入步骤

1. 下载 Spine Godot Extension 插件，放入项目根目录
2. 将 `.atlas`、`.skel`、`.png` 文件放入项目
3. 创建 `SpineSkeletonDataResource`，设置 Atlas Res 和 SkeletonFile Res
4. **禁用**项目设置中的"将文本资源转换为二进制"

### 转换工具

如需转换 Spine 版本: https://github.com/wang606/SpineSkeletonDataConverter

## 使用 3D 模型

```
MyCharacter (Node2D)
├── SubViewportContainer
│   └── SubViewport (transparent = true)
│       ├── Camera3D
│       └── 3D模型节点
├── Bounds (Control) %
├── IntentPos (Marker2D) %
└── CenterPos (Marker2D) %
```

设置 SubViewport 的 `transparent = true` 以正确渲染透明背景。

## 项目设置

**必须禁用**: 项目 → 项目设置 → 将文本资源转换为二进制

这是为了确保场景文件能被正确加载。
