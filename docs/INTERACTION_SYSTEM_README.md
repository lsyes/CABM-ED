# 交互意愿系统 - 完整实现

## ✅ 已完成的功能

### 1. 核心系统
- ✅ 交互意愿判定系统
- ✅ 存档系统（自动保存）
- ✅ 角色数据管理
- ✅ 失败反馈机制
- ✅ 冷却时间系统

### 2. UI 显示
- ✅ 左侧边栏显示角色状态
  - 好感度
  - 交互意愿
  - 心情（带颜色）
  - 精力
  - 信任等级
- ✅ 失败消息提示
- ✅ 自动保存提示

### 3. 自动保存
- ✅ 每5分钟自动保存
- ✅ 保存时间戳记录
- ✅ 多槽位支持

## 📁 文件结构

```
项目根目录/
├── config/
│   ├── interaction_config.json      # 交互配置
│   └── save_data_template.json      # 存档模板
├── scripts/
│   ├── interaction_manager.gd       # 交互管理器
│   ├── save_manager.gd              # 存档管理器
│   ├── interaction_test.gd          # 测试脚本
│   ├── main.gd                      # 主场景（已集成）
│   └── sidebar.gd                   # 侧边栏（已集成）
├── docs/
│   ├── interaction_system_guide.md  # 详细使用指南
│   ├── save_system_guide.md         # 存档系统指南
│   ├── interaction_system_summary.md # 系统总结
│   ├── setup_autoload.md            # 自动加载配置
│   └── quick_start_guide.md         # 快速启动指南
└── project.godot                     # 已添加自动加载配置
```

## 🎮 交互机制

### 成功概率公式
```
实际成功率 = (基础意愿 + 当前交互意愿 - 100)% + 修正因子
```

### 动作类型

| 动作ID | 名称 | 基础意愿 | 失败反馈 | 触发方式 |
|--------|------|----------|----------|----------|
| chat | 聊天 | 150% | 显示消息 | 点击聊天按钮 |
| click_character | 点击角色 | 120% | 显示消息 | 点击角色 |
| gift | 送礼物 | 200% | 显示消息 | 送礼按钮 |
| pat_head | 摸头 | 100% | 显示消息 | 摸头按钮 |
| call_name | 呼唤名字 | 90% | 显示消息 | 呼唤按钮 |
| enter_scene | 进入场景 | 80% | 无事发生 | **自动触发** |
| leave_scene | 离开场景 | 40% | 无事发生 | **自动触发** |

### 修正因子

**心情修正**:
- happy: +20%
- excited: +30%
- normal: 0%
- sad: -20%
- angry: -40%

**精力修正**:
- high (80-100): +10%
- normal (40-79): 0%
- low (20-39): -15%
- exhausted (0-19): -30%

## 🚀 快速开始

### 1. 重启编辑器
**必须重启 Godot 编辑器**，让自动加载配置生效。

### 2. 验证配置
打开 **项目 > 项目设置 > 自动加载**，确认：
- SaveManager
- InteractionManager

### 3. 运行游戏
按 F5 运行，左侧边栏会显示角色状态。

## 💡 使用示例

### 尝试交互
```gdscript
var interaction_mgr = get_node("/root/InteractionManager")
var success = interaction_mgr.try_interaction("chat")
if success:
    # 开始聊天
    start_chat()
```

### 修改数据
```gdscript
var save_mgr = get_node("/root/SaveManager")
save_mgr.add_affection(10)  # 增加好感度
save_mgr.set_mood("happy")  # 设置心情
```

### 查询状态
```gdscript
var save_mgr = get_node("/root/SaveManager")
var interaction_mgr = get_node("/root/InteractionManager")

print("交互意愿: ", save_mgr.get_reply_willingness())
print("聊天成功率: ", interaction_mgr.calculate_success_chance("chat") * 100, "%")
```

## 🧪 测试工具

使用 `interaction_test.gd` 脚本：
1. 附加到任意节点
2. 运行游戏
3. 按数字键测试各种功能

按键说明：
- `1` - 测试聊天
- `2` - 测试点击角色
- `4` - 查看当前状态
- `5/6` - 增加/减少交互意愿
- `7/8` - 设置好/坏心情
- `9` - 重置数据
- `0` - 显示所有成功率

## ⚙️ 配置调整

### 修改基础意愿
编辑 `config/interaction_config.json`:
```json
{
  "actions": {
    "chat": {
      "base_willingness": 150  // 改这里
    }
  }
}
```

### 修改初始数值
编辑 `config/save_data_template.json`:
```json
{
  "save_slots": {
    "slot_1": {
      "character_data": {
        "affection": 0,           // 初始好感度
        "reply_willingness": 100  // 初始交互意愿
      }
    }
  }
}
```

### 修改自动保存间隔
编辑 `scripts/sidebar.gd`:
```gdscript
auto_save_timer.wait_time = 300.0  // 秒数（300 = 5分钟）
```

## 📊 数据说明

### 角色数据
- **好感度** (0-100): 影响角色对玩家的态度
- **交互意愿** (0-100): 影响所有交互的成功率
- **心情**: happy, excited, normal, sad, angry
- **精力** (0-100): 影响交互成功率
- **信任等级** (0-100): 解锁特殊内容

### 存档位置
- Windows: `%APPDATA%/Godot/app_userdata/CABM-ED/saves/`
- Linux: `~/.local/share/godot/app_userdata/CABM-ED/saves/`
- macOS: `~/Library/Application Support/Godot/app_userdata/CABM-ED/saves/`

## 🔍 调试技巧

### 查看控制台
运行时观察输出：
```
存档模板已加载
交互配置已加载
动作: chat 成功率: 130% 掷骰: 0.45 结果: 成功
自动保存完成
```

### 常见问题

**Q: 看不到角色状态？**
A: 重启 Godot 编辑器

**Q: 交互总是失败？**
A: 检查交互意愿值，可能太低了

**Q: 想要100%成功？**
A: 设置交互意愿为100，心情为happy

## 📚 详细文档

- [快速启动指南](docs/quick_start_guide.md) - 新手必读
- [交互系统指南](docs/interaction_system_guide.md) - 完整API文档
- [存档系统指南](docs/save_system_guide.md) - 存档管理
- [系统总结](docs/interaction_system_summary.md) - 技术细节

## 🎯 下一步扩展

1. **添加更多动作** - 在配置文件中添加
2. **实现好感度增长** - 成功交互时增加
3. **添加事件系统** - 特定条件触发事件
4. **实现时间系统** - 根据时间调整意愿
5. **添加成就系统** - 记录玩家行为

## ✨ 特性亮点

- 🎲 **概率系统** - 真实的随机判定
- 💾 **自动保存** - 不用担心数据丢失
- 📊 **实时显示** - 左侧边栏实时更新
- 🎨 **颜色反馈** - 数值高低用颜色区分
- ⏱️ **冷却机制** - 防止频繁尝试
- 🔧 **易于配置** - JSON配置文件
- 🧪 **测试工具** - 快速调试

---

**系统已完全可用！** 重启编辑器后即可开始使用。

如有问题，请查看详细文档或控制台输出。
