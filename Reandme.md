# Rook

本地优先的 Windows 桌面 agent。它把“聊天”和“真正操作电脑”放在同一个工具里：对话模式只处理文字；工作模式可以看屏幕、操作窗口、读写文件、运行 PowerShell，并把每一步执行记录留在会话里。

Rook 不绑定单一模型厂商。可以保存多个接口，在 DeepSeek、Kimi、MiniMax、智谱 GLM、OpenAI、Anthropic、Gemini、本地模型之间切换；聊天模型和图像模型也可以分开配置。

| 工作模式 · 深色 | 工作模式 · 浅色 | 对话模式 |
| --- | --- | --- |
| ![工作模式深色](docs/screenshots/rook-dark.png) | ![工作模式浅色](docs/screenshots/rook-light.png) | ![对话模式](docs/screenshots/rook-chat.png) |

## 功能

- **对话 / 工作双模式**：对话模式不读屏幕、不执行命令；工作模式才开放桌面工具和任务台
- **真实的桌面操作**：屏幕截图、Windows OCR、窗口聚焦、键鼠输入、打开应用、文件读写、PowerShell
- **可检查的执行过程**：计划、工具调用、参数、输出、耗时和截图都留在本机会话里
- **两种权限**：动手前逐项确认，或自动执行；任何任务都可以随时停止
- **多接口模型库**：保存多个提供商，每个接口单独保存 Key；模型支持勾选添加、全部添加、从接口拉取
- **模型分工**：任务模型和图像模型独立配置，例如 DeepSeek 做任务、MiniMax 或 Seedream 生成图片
- **本地模型**：Ollama、LM Studio、vLLM、llama.cpp 预设，本机地址无需 Key
- **屏幕共享**：工作模式持续显示屏幕预览，新任务自动带上当前屏幕 OCR；通话中会收成浮动卡片
- **语音输入与朗读**：Windows 本机识别；微软 Edge 神经音色在线朗读，内置十余种中英文音色
- **语音通话**：电话按钮进入通话界面，说话自动发送，回答时暂停听音，结束后继续听
- **插件**：本地文件夹安装，注册新工具；示例插件提供系统快照
- **本地优先**：会话、工具记录、截图和生成图片保存在本机；API Key 使用系统密钥库保存

## 两种模式

### 对话

纯文本聊天，不读取屏幕、不调用桌面工具。适合解释代码、整理思路、读一段材料。

### 工作

开放桌面工具和任务台。可以截图识别屏幕、操作窗口、执行命令、修改文件；右侧任务台显示计划、屏幕和运行记录。

## 模型接入

设置 → 接口与模型 → **已添加的接口**。

1. 选提供商，填写 Base URL
2. 填 API Key
3. 在模型选项里勾选要加入的模型，或者点“全部添加”
4. 点“保存更改”，接口会出现在上方列表
5. 在右上角模型菜单里按接口分组选择模型

当前内置的常用模型包括：

- **DeepSeek**：V4.1 Flash（API 名 `deepseek-flash`）、V4 Pro（`deepseek-v4-pro`）
- **Kimi**：`kimi-k3.1-pro`、`kimi-k3.1-pro-highspeed`、`kimi-k3`
- **MiniMax**：`MiniMax-M3.1-Flash`、`MiniMax-M3`；图像模型 `image-01`、`image-01-live`
- **智谱 GLM**：`glm-5.3`、`glm-5.3-flash`、`glm-5.2`
- **OpenAI / Anthropic / Gemini / xAI Grok / Groq / Mistral / OpenRouter / SiliconFlow / Together**
- **本地**：Ollama、LM Studio、vLLM、llama.cpp

模型名会随官方更新，接入后点“拉取模型列表”以接口实际返回为准。

### 图像模型

工作模式里说“画一张……”，Rook 会调用 `generate_image`，用“设置 → 图像”里配置的接口生成图片。图片保存在本机，工具记录会自动展开预览。

支持：

- OpenAI Images 兼容接口
- Seedream / 火山方舟
- MiniMax 图像
- 自定义 OpenAI Images 兼容接口

## 屏幕共享与语音

### 屏幕共享

工作模式输入框下方点“共享屏幕”：

- 任务台持续刷新主显示器预览
- 下一轮任务自动附带当前屏幕和前台窗口的 OCR 文本
- 通话中共享屏幕时，通话界面会缩成右下角浮动卡片，避免挡住共享内容

### 语音

- 输入框右侧麦克风使用 Windows 本机语音识别
- 设置 → 执行行为 → 语音，可以切换音色并试听
- 在线朗读使用 Microsoft Edge 的 Read Aloud 神经音色，失败时回退到 Windows 本机语音
- 电话按钮打开通话界面，支持静音、共享屏幕、声音开关和结束通话

## 安装

### 安装程序

从 [Releases](https://github.com/JIEFANG-BWL/ROOK-agent/releases) 下载 `Rook-Setup-*.exe`，双击按向导安装。当前用户安装，不需要管理员权限。

当前安装包**没有代码签名**，Windows 可能显示“未知发布者”；需要点“更多信息 → 仍要运行”。

### 从源码运行

要求：Windows 10/11、Node.js 22+。

```powershell
git clone https://github.com/JIEFANG-BWL/ROOK-agent.git
cd ROOK-agent
npm install
npm start
```

如果项目在非系统盘（例如 `E:`），启动脚本会先把 Electron 运行时缓存到 `%LOCALAPPDATA%\Rook\runtime` 再启动。

### 构建

```powershell
# 便携版
npm run build

# NSIS 安装程序
npx electron-builder --win nsis --x64
```

安装程序输出到 `dist/installer/Rook-Setup-*.exe`。

## 目录结构

```text
main/                Electron 主进程、模型接口、工具、语音、屏幕共享
preload/             安全 IPC 桥
renderer/            界面、对话、设置、任务台
scripts/             安装、截图、OCR、语音和打包脚本
assets/              字体
examples/            示例插件
test/                单元测试和本地 mock 测试
docs/                截图和签名说明
electron-builder.yml NSIS 安装包配置
```

技术栈：Electron 44、原生 JavaScript/CSS、PowerShell、Windows OCR、OpenAI Chat Completions、Anthropic Messages。

## 安全与隐私

- 会话、计划、工具输出、截图和生成图片保存在 `%APPDATA%\Rook`
- API Key 使用 Windows `safeStorage` 加密，保存在本机，不写入会话记录
- 没有云端账号系统，也没有远程会话同步
- 工作模式会真实控制本机。第一次使用建议保持“动手前确认”
- 插件是运行在本机的 Node 代码，只安装可信来源

## 已知限制

- 仅支持 Windows
- 语音识别目前使用 Windows 本机引擎，安静环境效果更好
- 在线朗读需要网络
- 屏幕共享以主显示器为主，暂未提供按窗口选择
- 同一提供商当前保存一把 Key
- 安装包未做代码签名
- 暂不支持视频生成

## English

Rook is a local-first Windows desktop agent. It separates conversation from action: Conversation mode is text-only, while Work mode can capture and OCR the screen, control windows, edit files, run PowerShell, and keep every tool call visible in the session.

It supports multiple model providers with per-provider keys, curated multi-model selection, and separate roles for chat and image generation. DeepSeek, Kimi, MiniMax, Zhipu GLM, OpenAI, Anthropic, Gemini, Groq, Mistral, and local servers such as Ollama, LM Studio, vLLM, and llama.cpp are supported.

Rook also provides live screen sharing, Windows speech input, Edge neural voice output, and a call-style voice interface. Sessions, tool logs, screenshots, and generated images stay on the local machine; API keys are stored with the Windows credential store.

**Requirements:** Windows 10/11, Node.js 22+ for source builds.

```powershell
git clone https://github.com/JIEFANG-BWL/ROOK-agent.git
cd ROOK-agent
npm install
npm start
```

**Limitations:** Windows only; local Windows speech recognition; online TTS; primary-display screen sharing; one key per provider; unsigned installer; no video generation.

## License

MIT. See [LICENSE](LICENSE).

第三方字体和语音服务说明见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
