# 快速启动指南

## ✅ 已完成的配置

所有必要的文件和配置都已创建完成！

## 🚀 启动步骤

### 1. 重启 Godot 编辑器

**重要**: 必须重启 Godot 编辑器，让自动加载配置生效。

关闭并重新打开 Godot 项目。

### 2. 验证自动加载

在 Godot 编辑器中：
1. 打开 **项目 > 项目设置**
2. 选择 **自动加载** 标签
3. 确认看到以下两个节点：
   - `SaveManager` → `res://scripts/save_manager.gd`
   - `InteractionManager` → `res://scripts/interaction_manager.gd`

### 3. 运行游戏

点击 **运行项目** (F5) 或 **运行当前场景** (F6)

## 📊 查看角色状态

启动游戏后，在左侧边栏可以看到：
- ⏰ 电子时钟
- 📈 角色状态：
  - 好感度: 0
  - 交互意愿: 100
  - 心情: 普通
  - 精力: 100
  - 信任: 0

## 🎮 测试交互系统

### 方法1: 游戏内测试
1. 点击角色 → 触发交互判定
2. 选择"聊天" → 触发聊天判定
3. 观察失败消息（如果交互失败）

### 方法2: 使用测试脚本
1. 在场景树中添加一个新节点（任意类型）
2. 附加脚本 `res://scripts/interaction_test.gd`
3. 运行游戏
4. 按数字键测试：
   - `1` - 测试聊天
   - `2` - 测试点击角色
   - `3` - 测试进入场景
   - `4` - 查看当前状态
   - `5` - 增加交互意愿 (+10)
   - `6` - 减少交互意愿 (-10)
   - `7` - 设置好心情
   - `8` - 设置坏心情
   - `9` - 重置所有数据
   - `0` - 显示所有动作成功率

## 💾 自动保存

游戏会每 **5分钟** 自动保存一次。

手动保存：
```gdscript
get_node("/root/SaveManager").save_game()
```

## 🎯 调整交互概率

编辑 `config/interaction_config.json` 文件：

```json
{
  "actions": {
    "chat": {
      "base_willingness": 150  // 修改这个值调整基础概率
    }
  }
}
```

## 📝 常见问题

### Q: 看不到角色状态显示？
A: 确保已重启 Godot 编辑器，让自动加载生效。

### Q: 点击角色没反应？
A: 检查控制台是否有错误信息。确认自动加载配置正确。

### Q: 想修改初始数值？
A: 编辑 `config/save_data_template.json` 文件。

### Q: 如何查看存档文件？
A: 存档位置：
- Windows: `%APPDATA%/Godot/app_userdata/CABM-ED/saves/`
- Linux: `~/.local/share/godot/app_userdata/CABM-ED/saves/`
- macOS: `~/Library/Application Support/Godot/app_userdata/CABM-ED/saves/`

## 🔧 调试技巧

### 查看控制台输出
运行游戏时，观察控制台输出：
```
存档模板已加载
交互配置已加载
动作: chat 成功率: 130% 掷骰: 0.45 结果: 成功
```

### 手动测试交互
在任意脚本中：
```gdscript
# 获取管理器
var save_mgr = get_node("/root/SaveManager")
var interaction_mgr = get_node("/root/InteractionManager")

# 查看数据
print("交互意愿: ", save_mgr.get_reply_willingness())
print("聊天成功率: ", interaction_mgr.calculate_success_chance("chat"))

# 修改数据
save_mgr.set_reply_willingness(30)
save_mgr.set_mood("angry")

# 测试交互
interaction_mgr.try_interaction("chat")
```

## 📚 更多文档

- [交互系统详细指南](interaction_system_guide.md)
- [存档系统指南](save_system_guide.md)
- [系统总结](interaction_system_summary.md)

## ✨ 下一步

现在系统已经完全可用！你可以：
1. 调整配置文件中的数值
2. 添加更多动作类型
3. 实现好感度增长逻辑
4. 添加更多交互反馈
5. 根据游戏进度动态调整意愿

祝开发顺利！🎉
