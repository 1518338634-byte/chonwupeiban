# 棉花伙伴 - iOS 情绪解压应用设计文档

## 1. 产品概述

### 1.1 产品定位
一款以 3D 棉花糖小精灵为核心角色的情绪陪伴类 iOS 应用。用户通过与 AI 小精灵对话，获得温暖的情绪共情与陪伴，帮助缓解日常压力与负面情绪。

### 1.2 核心理念
- **陪伴而非治疗**：小精灵是朋友，不是心理咨询师
- **共情优先**：先接住情绪，再考虑建议
- **轻量自然**：像朋友发微信一样简短温暖

### 1.3 目标用户
- 18-35 岁年轻人，日常面临学业/工作/人际压力
- 喜欢治愈系角色、需要情绪出口但不想找真人倾诉的用户
- 轻度情绪困扰，不需要专业心理干预

---

## 2. 核心功能

### 2.1 AI 小精灵对话（主功能）

| 项目 | 说明 |
|------|------|
| 交互方式 | 文字聊天，类似微信对话界面 |
| AI 角色 | 温柔、黏人的棉花糖小精灵伙伴，名字由用户自定义 |
| 回复风格 | 1-2 句话为主，最多 3 句，简短自然 |
| 情绪识别 | AI 自动识别用户情绪（开心/平静/难过/焦虑/愤怒） |
| 角色动作 | 约 20% 回复自然带入小精灵身份动作（蹭蹭、拍拍你、拉你的手、揉揉脸、抱住你等） |

#### 情绪应对策略

| 情绪 | 策略 |
|------|------|
| 开心 | 一起开心，放大快乐，追问好事 |
| 平静 | 安静陪伴，营造舒适感 |
| 难过 | 先接住情绪，允许难过，不催促振作 |
| 焦虑 | 承认焦虑合理，给可执行的小步骤 |
| 愤怒 | 先站用户这边，等缓和后再柔和建议 |

### 2.2 角色形象与 3D 场景系统

#### 两套小精灵形象（基于设计稿）

应用提供 **两个小精灵角色**，用户首次进入时选择其一，后续可随时更换：

| 形象 | 配色 | 性格特征 | 3D 场景 | 氛围 |
|------|------|----------|---------|------|
| **暖暖** | 粉色/暖色系 | 温柔体贴、安静治愈，喜欢待在室内陪你 | 简洁 3D 暖色小屋（圆润家具、柔光窗户、漂浮星星） | 安全感、治愈、放松 |
| **元气** | 蓝色/冷色系 | 活泼开朗、元气满满，喜欢带你出去走走 | 简洁 3D 户外小岛（低多边形草地、云朵、彩虹桥） | 清新、自由、活力 |

> 设计参考：`cf3a1d76e37fd73fdfad2a0b1cae3ef7165a967ec9911-UaJrzt.jpeg`

#### 3D 视觉风格定义

| 项目 | 规范 |
|------|------|
| 整体风格 | 简洁 3D、低多边形 + 圆润造型，类似 Monument Valley / Lofi Girl 3D 风 |
| 配色 | 低饱和莫兰迪色系，柔和不刺眼 |
| 材质 | 哑光/磨砂质感，无复杂纹理 |
| 光影 | 柔和全局光照 + 微弱环境光，无硬阴影 |
| 场景元素 | 极简几何体（圆柱、球体、圆角方块），不超过 10 个主要物件 |
| 角色建模 | 圆胖 Q 萌比例（头身比约 1:1），四肢短小圆润，无锐角 |

#### 首次进入 - 形象选择引导

```
┌─────────────────────────────────┐
│        选择你的小伙伴             │
│                                 │
│  ┌─────────┐   ┌─────────┐     │
│  │         │   │         │     │
│  │  暖暖    │   │  元气    │     │
│  │ (粉色)  │   │ (蓝色)  │     │
│  └────┬────┘   └────┬────┘     │
│       │              │          │
│  "我陪你待着"    "我带你走走"     │
│                                 │
│     点击选择，随时可以更换         │
└─────────────────────────────────┘
```

**引导流程：**
1. 欢迎页 → 3D 场景中两个小精灵并排站立，向用户招手
2. 用户左右滑动或点击选择心仪的小精灵
3. 为小精灵取名（输入框 + 随机名字推荐）
4. 小精灵开心跳跃 + 拥抱动画 → 进入首页

#### 形象更换机制

- **入口**：设置页 → 「更换小伙伴」
- **更换时**：3D 转盘展示两个角色，当前使用的标记高亮
- **更换后**：名字保留不变，对话历史保留，仅切换角色和场景
- **切换动画**：当前小精灵挥手告别 → 新小精灵从远处跑来拥抱的过渡动画

#### 形象与场景联动

每套形象包含完整的 3D 视觉资源包：

| 资源 | 暖暖（粉色） | 元气（蓝色） |
|------|-------------|-------------|
| 首页 3D 场景 | 暖色小屋内景（沙发、窗台、星星灯） | 户外小岛（草地、云朵、彩虹桥） |
| 待机动画 | 坐在沙发上晃脚、偶尔打哈欠 | 在草地上蹦跳、追云朵 |
| 聊天页头像 | 粉色系 3D 渲染头像 | 蓝色系 3D 渲染头像 |
| 互动模式背景 | 小屋地毯上 | 草地花丛中 |
| 情绪动画集 | 室内版 5 种情绪动画 | 户外版 5 种情绪动画 |

#### 情绪动画对照

| 情绪 | 暖暖（粉色） | 元气（蓝色） |
|------|-------------|-------------|
| 开心 | 坐在沙发上拍手、脸颊泛红、脚丫开心晃动 | 在草地上转圈跳舞、张开双手拥抱天空 |
| 平静 | 靠在窗边闭眼微笑、身体微微起伏呼吸 | 躺在云朵上悠闲晃腿、望着天空发呆 |
| 难过 | 蜷缩在沙发角落、低头抱膝、身体微微颤抖 | 蹲在草地上、双手捂脸、身边花朵也低垂 |
| 焦虑 | 在房间里小碎步来回走、双手搓在一起 | 在小岛边缘来回踱步、不安地东张西望 |
| 愤怒 | 鼓起腮帮子、双手叉腰、跺脚 | 使劲跺地面、双拳握紧、脸涨红 |

### 2.3 情绪日记（辅助功能）

- 自动记录每次对话的情绪标签
- 生成每周/每月情绪趋势图
- 用户可回顾历史对话

### 2.4 解压小工具

| 工具 | 说明 |
|------|------|
| 互动模式 | 触摸屏幕与小精灵互动（摸头、戳脸、拉手），触发不同反应动画和触觉反馈 |
| 白噪音 | 雨声、壁炉声、海浪声、森林鸟鸣等环境音 |
| 呼吸引导 | 小精灵带你做呼吸练习（吸气-小精灵身体膨胀发光，呼气-缓缓缩小变柔和） |
| 情绪瓶子 | 把烦恼写下来"装进瓶子"，小精灵帮你"抱着守护" |

---

## 3. 技术架构

### 3.1 整体架构

```
┌──────────────────────────────────────────────┐
│                iOS App (Swift)                │
├──────────────┬──────────────┬────────────────┤
│   UI Layer   │  3D Engine   │     Audio      │
│  (SwiftUI)   │ (SceneKit)   │ (AVFoundation) │
├──────────────┴──────────────┴────────────────┤
│              Business Logic                   │
│    (Chat Manager / Emotion Engine)            │
├──────────────────────────────────────────────┤
│              Network Layer                    │
│    (API Client → 火山引擎 ARK API)            │
├──────────────────────────────────────────────┤
│              Local Storage                    │
│          (SwiftData / Core Data)             │
└──────────────────────────────────────────────┘
```

### 3.2 技术选型

| 层级 | 技术方案 |
|------|----------|
| UI 框架 | SwiftUI (iOS 16+) |
| 3D 渲染 | SceneKit（轻量 3D 场景） + 预渲染 Lottie 动画（情绪表情） |
| 3D 建模格式 | USDZ / glTF（Apple 原生支持） |
| AI 后端 | 火山引擎 ARK API（豆包 doubao-seed-2-0-lite 模型） |
| 网络层 | URLSession + async/await |
| 本地存储 | SwiftData |
| 音频 | AVFoundation |
| 触觉反馈 | Core Haptics |
| 推送 | APNs (本地通知为主) |

### 3.3 3D 场景技术方案

```swift
// 场景管理
enum CompanionType: String, Codable {
    case warm   // 暖暖（粉色）
    case energy // 元气（蓝色）
}

struct SceneConfig {
    let companionModel: String   // "warm_companion.usdz" / "energy_companion.usdz"
    let sceneModel: String       // "warm_room.usdz" / "island_outdoor.usdz"
    let emotionAnimations: [Emotion: String]  // 情绪 → 动画文件映射
    let ambientColor: Color      // 环境光色调
}
```

### 3.4 AI 调用方案

基于火山引擎 ARK 平台 Responses API，使用豆包大模型。

```swift
// API 配置
struct ARKConfig {
    static let baseURL = "https://ark.cn-beijing.volces.com/api/v3/responses"
    static let model = "doubao-seed-2-0-lite-260215"
    static let apiKey = "Bearer <your-ark-api-key>"  // 从 Keychain 安全读取
}

// 请求结构（兼容 ARK Responses API 格式）
struct ARKRequest: Encodable {
    let model: String = ARKConfig.model
    let input: [ARKMessage]
}

struct ARKMessage: Encodable {
    let role: String  // "system" / "user" / "assistant"
    let content: ARKContent
}

// 支持纯文本和多模态内容
enum ARKContent: Encodable {
    case text(String)
    case multipart([ARKContentPart])

    func encode(to encoder: Encoder) throws {
        var container = encoder.singleValueContainer()
        switch self {
        case .text(let str):
            try container.encode(str)
        case .multipart(let parts):
            try container.encode(parts)
        }
    }
}

struct ARKContentPart: Encodable {
    let type: String       // "input_text" / "input_image"
    let text: String?
    let image_url: String?
}

// AI 返回格式（由 system prompt 约束）
struct ChatResponse: Codable {
    let emotion: String  // 开心/平静/难过/焦虑/愤怒
    let reply: String    // 小精灵回复内容
}
```

**调用示例（Swift async/await）：**

```swift
func sendMessage(_ userText: String, history: [ARKMessage]) async throws -> ChatResponse {
    let systemMessage = ARKMessage(
        role: "system",
        content: .text(SystemPrompt.content)  // system_prompt.md 内容
    )
    let userMessage = ARKMessage(
        role: "user",
        content: .text(userText)
    )

    let request = ARKRequest(input: [systemMessage] + history + [userMessage])

    var urlRequest = URLRequest(url: URL(string: ARKConfig.baseURL)!)
    urlRequest.httpMethod = "POST"
    urlRequest.setValue(ARKConfig.apiKey, forHTTPHeaderField: "Authorization")
    urlRequest.setValue("application/json", forHTTPHeaderField: "Content-Type")
    urlRequest.httpBody = try JSONEncoder().encode(request)

    let (data, _) = try await URLSession.shared.data(for: urlRequest)
    // 从 ARK 响应中提取 output text，再解析为 ChatResponse JSON
    let arkResponse = try JSONDecoder().decode(ARKResponseWrapper.self, from: data)
    let chatResponse = try JSONDecoder().decode(ChatResponse.self, from: arkResponse.outputText.data(using: .utf8)!)
    return chatResponse
}
```

### 3.5 Token 消耗与成本估算

| 项目 | 数值 |
|------|------|
| System Prompt | ~1200 tokens |
| 单次请求总消耗 | ~1300-1400 tokens |
| 日均对话次数（预估） | 20-50 次/用户 |
| 单用户日成本（豆包 Lite） | 极低（豆包 Lite 定价远低于海外模型） |

> 豆包 doubao-seed-2-0-lite 为轻量模型，适合高频短对话场景，成本优势明显。

---

## 4. 页面结构

### 4.1 页面导航

```
App
├── 首次引导流程（仅首次启动）
│   ├── 欢迎页（3D 双角色展示）
│   ├── 形象选择页（暖暖 / 元气）
│   └── 取名页
├── 首页（3D 场景 + 小精灵 + 对话入口）
├── 聊天页（核心对话界面）
├── 解压工具页
│   ├── 互动模式（摸头/戳脸/拉手）
│   ├── 白噪音
│   ├── 呼吸引导
│   └── 情绪瓶子
├── 情绪日记页
│   ├── 日历视图
│   └── 趋势图表
└── 设置页
    ├── 小精灵名字
    ├── 更换小伙伴（暖暖 ↔ 元气）
    ├── 通知设置
    └── 隐私与数据
```

### 4.2 核心页面设计

#### 首页
- 全屏 3D 场景渲染，小精灵居中（占屏幕 60%）
- 场景背景随时间变化（白天柔光/傍晚暖橘/夜晚星空）
- 小精灵根据当前时间/最近情绪展示不同待机动画
- 支持手势旋转/缩放 3D 场景（轻度交互）
- 底部对话输入框，点击进入聊天页
- 小精灵偶尔主动"说话"（气泡弹出 + 本地通知）

#### 聊天页
- 类微信气泡式对话界面
- 小精灵 3D 渲染头像 + 用户头像
- 顶部小型 3D 小精灵（随情绪实时切换表情动画）
- 输入框支持文字，后续可扩展语音

#### 互动模式
- 全屏 3D 小精灵，支持多点触控
- 摸头 → 开心眯眼 + 脸红
- 戳脸 → 鼓腮帮子 + 轻微晃动
- 拉手 → 害羞低头 + 跟着手指走
- 拍肩 → 转头看你 + 微笑
- Core Haptics 模拟触摸反馈（轻柔震动）
- 背景播放舒缓环境音

---

## 5. 安全与合规

### 5.1 心理安全机制

- 当用户提到自杀、自伤等关键词时，AI 回复中自然融入心理援助热线 **12320-5**
- 不替代专业心理咨询，App 内明确声明
- 设置页提供心理援助资源链接

### 5.2 数据隐私

- 对话数据仅存储在本地设备
- 发送给 AI 的消息不包含用户身份信息
- 符合 Apple App Store 隐私政策
- 提供一键清除所有数据功能

### 5.3 内容安全

- AI 绝不提供医疗/心理诊断建议
- 禁止生成有害、暴力、色情内容
- 回复长度严格限制，避免信息过载

---

## 6. 商业模式

### 6.1 免费功能
- 每日 20 次 AI 对话
- 两个基础小精灵形象（暖暖/元气）自由切换
- 互动模式
- 情绪日记

### 6.2 订阅功能（Pro）

| 权益 | 说明 |
|------|------|
| 无限对话 | 不限次数与 AI 小精灵聊天 |
| 更多装扮 | 解锁小精灵配饰（帽子、围巾、背包、翅膀等） |
| 更多场景 | 解锁额外 3D 场景（海边、太空、糖果屋等） |
| 全部解压工具 | 白噪音、呼吸引导、情绪瓶子 |
| 深度情绪报告 | 月度情绪分析与建议 |

### 6.3 定价建议
- 月订阅：18 元/月
- 年订阅：128 元/年（约 10.7 元/月）

---

## 7. 开发计划

### Phase 1 - MVP（5 周）
- [x] 基础聊天功能（AI 对话）
- [x] 情绪识别与回复
- [x] 本地对话存储
- [ ] 两个小精灵 3D 模型制作（暖暖/元气）
- [ ] 两套 3D 场景搭建（小屋/小岛）
- [ ] SceneKit 场景渲染集成
- [ ] 首次引导流程（形象选择 + 取名）

### Phase 2 - 体验优化（3 周）
- [ ] 5 种情绪动画 × 2 套角色（共 10 组）
- [ ] 场景昼夜光照变化
- [ ] 互动模式 + 触觉反馈
- [ ] 情绪日记与趋势图
- [ ] 小精灵主动问候（本地通知）
- [ ] 设置页形象更换功能

### Phase 3 - 功能扩展（3 周）
- [ ] 白噪音模块
- [ ] 呼吸引导
- [ ] 情绪瓶子
- [ ] 订阅系统接入
- [ ] 额外付费场景/配饰资源

### Phase 4 - 上线打磨（2 周）
- [ ] App Store 审核准备
- [ ] 3D 渲染性能优化（低端机型适配）
- [ ] 用户测试与反馈迭代
- [ ] 隐私政策与用户协议

---

## 8. 关键指标（KPI）

| 指标 | 目标 |
|------|------|
| 日活跃用户 (DAU) | 上线 3 个月达 5000 |
| 次日留存率 | > 40% |
| 7 日留存率 | > 25% |
| 日均对话轮次 | > 10 轮/活跃用户 |
| 付费转化率 | > 5% |
| App Store 评分 | > 4.5 |

---

## 9. 风险与应对

| 风险 | 应对方案 |
|------|----------|
| AI 回复不当 | 严格 system prompt + 内容过滤层 |
| API 成本过高 | 使用豆包 Lite 模型 + 对话次数限制 + 合理 max_tokens |
| 3D 渲染性能 | SceneKit 轻量方案 + LOD 分级 + 低端机降级为 2D 预渲染 |
| 用户依赖过度 | 适时提醒线下社交，提供专业资源入口 |
| App Store 审核 | 提前准备隐私政策，明确非医疗声明 |
| 竞品模仿 | 持续优化角色人格与 3D 交互体验差异化 |

---

## 附录

### System Prompt

详见 [system_prompt.md](./system_prompt.md)。

> 注意：system_prompt.md 当前仍为猫咪角色版本，上线前需适配为小精灵角色，将猫咪动作（蹭蹭、打呼噜、肉垫等）替换为小精灵动作（拍拍你、拉你的手、揉揉脸、抱住你等）。

### API 参考

详见 [API.md](./API.md)，火山引擎 ARK Responses API 调用示例。

- **Endpoint**: `https://ark.cn-beijing.volces.com/api/v3/responses`
- **Model**: `doubao-seed-2-0-lite-260215`
- **支持多模态**：可传入 `input_image` + `input_text`（预留未来图片情绪识别能力）

---

*文档版本：v2.0 | 更新日期：2026-04-18*
