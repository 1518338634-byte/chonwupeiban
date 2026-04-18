# 猫咪伙伴 AI 

---

## 一、项目背景

这是「情绪岛」App 中的**猫咪伙伴（AI陪伴对话）** 功能的训练数据 + prompt 工程产出物。

**产品定位：** 面向18-30岁年轻人的情绪疗愈 App，核心 IP 是一只温柔的猫咪，通过对话陪伴用户处理开心、平静、难过、焦虑、愤怒五种情绪状态。

**当前技术方案：** Prompt Engineering（不做 fine-tuning，时间不够）
- 直接调用大模型 API（现有项目用 OpenRouter → Claude 3.5 Haiku）
- 用强力 system prompt + few-shot 示例控制风格
- 输出格式为 JSON：`{"emotion": "...", "reply": "..."}`

---

## 二、本次交接的产出物清单

```
猫咪伙伴AI交接包/
├── HANDOVER.md                  ← 本文件
├── system_prompt.md             ← 核心：猫咪伙伴 system prompt（直接可用）
├── all_training_data.jsonl      ← 合并后的完整训练数据（265条）
├── data/
│   ├── happy_50.jsonl           ← 开心场景单轮对话（50条）
│   ├── calm_50.jsonl            ← 平静场景单轮对话（50条）
│   ├── sad_50.jsonl             ← 难过场景单轮对话（50条）
│   ├── anxious_50.jsonl         ← 焦虑场景单轮对话（50条）
│   ├── angry_50.jsonl           ← 愤怒场景单轮对话（50条）
│   ├── multi_turn_happy.jsonl   ← 开心场景多轮对话（3组）
│   ├── multi_turn_calm.jsonl    ← 平静场景多轮对话（3组）
│   ├── multi_turn_sad.jsonl     ← 难过场景多轮对话（3组）
│   ├── multi_turn_anxious.jsonl ← 焦虑场景多轮对话（3组）
│   └── multi_turn_angry.jsonl   ← 愤怒场景多轮对话（3组）
└── app-debug.apk                ← 已打包的安卓 APK（catnest-app 项目）
```

---

## 三、训练数据说明

### 数据格式
OpenAI chat completion 格式（JSONL，每行一个 JSON 对象）：

```json
{"messages": [
  {"role": "system", "content": "你是一只温柔的猫咪伙伴..."},
  {"role": "user", "content": "用户输入"},
  {"role": "assistant", "content": "猫咪回复"}
]}
```

多轮对话则在同一个 messages 数组中交替 user/assistant：

```json
{"messages": [
  {"role": "system", "content": "..."},
  {"role": "user", "content": "第一句"},
  {"role": "assistant", "content": "回复1"},
  {"role": "user", "content": "第二句"},
  {"role": "assistant", "content": "回复2"}
]}
```

### 数据统计

| 类型 | 条数 | 说明 |
|------|------|------|
| 单轮对话 × 5场景 | 250条 | 每场景50条，覆盖多种子场景 |
| 多轮对话 × 5场景 | 15组 | 每场景3组，每组4轮对话 |
| **合计** | **265条** | `all_training_data.jsonl` |

### 猫咪风格核心规则（训练数据全部遵循）
- 语气亲近，像朋友发微信，1-2句话为主
- 共情优先，不说教，不强行正能量
- **约20%** 的回复自然融入猫咪身份特征（蹭蹭、尾巴、爪子、肉垫、耳朵等）
- 不是每句都有猫咪动作，分散自然地出现

---

## 四、System Prompt 说明

详见 `system_prompt.md`，可直接复制使用。

**核心结构：**
1. 角色定义（猫咪伙伴，陪伴共情为主）
2. 回复规则（长度、共情优先、建议方式、猫咪特征占比）
3. 5条绝对禁令
4. 5种情绪的应对策略（开心/平静/难过/焦虑/愤怒）
5. 安全兜底（自杀/自伤关键词 → 心理热线 12320-5）
6. 输出格式（严格 JSON）
7. 10条 few-shot 示例（覆盖全部5种情绪）

**输出格式：**
```json
{"emotion": "开心", "reply": "哇！！太厉害了吧！恭喜恭喜！"}
```
emotion 只能是：开心、平静、难过、焦虑、愤怒（5选1）

---

## 五、接入代码位置

现有 App（`catnest-app`，Vite + React + Capacitor）目前使用**关键词匹配**（无 AI），尚未接入 API。

如需接入 AI，参考路径：
- 前端代码：`C:\Users\Wing\Desktop\mood-island-project\catnest-app\src\`
- 前端交接文档：`C:\Users\Wing\Desktop\mood-island-project\catnest-app\FRONTEND_HANDOFF.md`
- API 接入规范：`C:\Users\Wing\Desktop\mood-island-project\catnest-app\BACKEND_API_SPEC.md`

另有一个 Expo React Native 版本（`frontend/`）已有完整 AI 对话实现，可参考其 `lib/api.ts` 的调用模式。

---

## 六、APK 说明

`app-debug.apk` 是 `catnest-app` 项目的 **Debug 版安卓安装包**。

- 打包时间：2026-04-09
- 包名：`com.catnest.app`
- 版本：1.0（versionCode 1）
- 安装方式：手机开启「允许安装未知来源应用」后直接安装
- 注意：Debug 版，仅供测试，**不能上架应用商店**

---

## 七、后续可能的任务

以下是用户原始需求中提到、本次尚未完成的方向：

1. **将 system prompt 接入 catnest-app**：替换现有关键词匹配逻辑，改为调用 AI API
2. **Fine-tuning（未来）**：`all_training_data.jsonl` 已准备好，可用于 OpenAI fine-tuning
3. **情绪标签同步**：现有代码情绪分类是4种（开心/平静/焦虑/悲伤），新 prompt 是5种（多了愤怒，"悲伤"改为"难过"），接入时需同步更新

---

*本交接包由上一位 AI 生成，日期：2026-04-09*
