# Summary Model 占位符支持

## 改进说明

现在 `summary_model` 的 `system_prompt` 支持使用占位符，可以让总结以角色的视角进行。

## 支持的占位符

在 `summary_model.system_prompt` 中可以使用以下占位符：

- `{character_name}` - 角色名称
- `{user_name}` - 用户名称
- `{user_address}` - 角色对用户的称呼

## 使用示例

### 客观总结（旧方式）
```json
"system_prompt": "你是一个总结专家。请将两人的对话总结成简短的描述，保持简洁，不超过50字。/no_think"
```

**效果**：生成客观的第三人称总结
- 示例：「用户和角色讨论了今天的天气，角色表示很喜欢晴天。」

### 角色视角总结（新方式）
```json
"system_prompt": "你是{character_name}。请以你的视角，用第一人称总结你和{user_address}的对话，保持简洁，不超过50字。/no_think"
```

**效果**：生成角色第一人称的总结
- 示例：「我和主人聊了今天的天气，我告诉他我很喜欢晴天。」

### 更多示例

#### 日记风格
```json
"system_prompt": "你是{character_name}。请以日记的形式，用第一人称记录你和{user_address}刚才的对话，不超过50字。/no_think"
```

#### 简短记录
```json
"system_prompt": "你是{character_name}。用一句话记录你刚才和{user_address}聊了什么。/no_think"
```

#### 情感记录
```json
"system_prompt": "你是{character_name}。总结你和{user_address}的对话，重点描述你的感受和情绪，不超过50字。/no_think"
```

## 技术实现

修改位置：`scripts/ai_service.gd` 中的 `_call_summary_api` 函数

```gdscript
# 获取角色和用户信息
var save_mgr = get_node("/root/SaveManager")
var char_name = save_mgr.get_character_name()
var user_name = save_mgr.get_user_name()
var user_address = save_mgr.get_user_address()

# 替换占位符
var system_prompt = summary_config.system_prompt
system_prompt = system_prompt.replace("{character_name}", char_name)
system_prompt = system_prompt.replace("{user_name}", user_name)
system_prompt = system_prompt.replace("{user_address}", user_address)
```

## 配置文件

修改 `config/ai_config.json` 中的 `summary_model.system_prompt` 即可。

## 注意事项

1. 占位符会在运行时自动替换，无需手动处理
2. 如果不使用占位符，保持原有的prompt也完全可以
3. 占位符区分大小写，必须完全匹配
4. 建议在prompt末尾保留 `/no_think` 以提高响应速度

## 与日记系统的配合

由于日记系统现在统一显示所有记录（对话和离线事件），使用角色视角的总结可以让日记更加连贯和有代入感。

**示例日记显示**：
```
💬 14:30
我和主人聊了今天的天气，我告诉他我很喜欢晴天。

⏰ 15:00
我在客厅看了一会儿书，然后去厨房准备了下午茶。

💬 16:20
主人问我想去哪里玩，我说想去公园散步。
```

这样的日记读起来就像是角色的私人日记，更有沉浸感。

## 相关文档

- `docs/diary_system_improvement.md` - 日记系统改进说明
- `config/ai_config.json` - AI配置文件
- `scripts/ai_service.gd` - AI服务实现
