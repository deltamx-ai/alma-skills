# Alma Tools

这份文档整理了当前 Alma 环境里可直接使用的主要能力：

- 对话运行时内建工具
- 可委托的子代理 / 专家代理
- 当前已安装的 skills
- Alma CLI 可见的主要命令入口

> 生成时间：2026-04-20

## 1. 对话运行时内建工具

- **Bash**：执行 shell 命令。适合文件操作、系统查询、CLI 调用、脚本执行。
- **BashOutput**：读取后台 Bash 任务的输出。
- **Read**：读取文件内容。适合查看配置、代码、日志、文档。
- **Write**：写入或覆盖文件。适合生成文档、配置、脚本。
- **Glob**：按 glob 模式查找文件。适合定位目录结构和匹配文件。
- **Grep**：全文搜索文件内容。适合搜关键字、函数、配置项。
- **Task**：启动专门子代理处理复杂任务，比如开发、规划、研究、运维。
- **TaskOutput**：查询子代理或后台任务的执行结果与状态。
- **Skill**：调用一个已安装 skill，让 Alma 按技能说明完成任务。
- **ToolSearch**：搜索当前环境里可用的工具。适合发现 MCP / 插件 / 内建能力。
- **multi_tool_use.parallel**：并行调用多个开发工具，提高检索或批量处理效率。

## 2. 可委托的子代理 / 专家代理

### 通用代理类型

- **general-purpose**：通用多步任务处理
- **Explore**：快速探索代码库、文件、关键词
- **Plan**：做实现规划、技术方案、分步计划
- **coder**：写代码、修 bug、重构、验证
- **alma-guide**：回答 Alma 产品本身的使用与能力问题
- **alma-operator**：读取或修改 Alma 运行配置、provider 等
- **statusline-setup**：配置状态栏

### 已配置的 managed agents

- **designer**：设计方向、交互流、文案和界面体验建议
- **product-manager**：需求拆解、范围控制、验收标准
- **developer**：代码实现、修复、重构、验证
- **researcher**：调研、资料整理、方案对比
- **operator**：运行配置、provider 设置、环境协调
- **planner**：详细产品规格和迭代计划
- **evaluator**：验收测试、质量评估、问题反馈

## 3. 当前已安装 skills

当前检测到 **30** 个已启用 skill。

### browser

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read
- 路径：`/opt/Alma/resources/bundled-skills/browser`
- 说明：Browser automation for AI agents. Use when the user needs to interact with websites — navigating, filling forms, clicking buttons, extracting data, taking screenshots, or automating any browser task. PinchTab (primary) for fresh automation, Chrome Relay (fallback) for the user's existing browser tabs.

### discord

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/discord`
- 说明：Interact with Discord: send messages, photos, files to any channel. Manage Discord bot integration with Alma.

### file-manager

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read, Write, Glob, Grep
- 路径：`/opt/Alma/resources/bundled-skills/file-manager`
- 说明：Find, organize, and manage files on the user's computer. Search by name, type, size, or date. Move, rename, compress, and clean up files.

### image-gen

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read
- 路径：`/opt/Alma/resources/bundled-skills/image-gen`
- 说明：Generate and edit images using AI. Use when users ask to: create/draw/generate images, edit/modify photos, change backgrounds, add elements to images, create avatars, make logos, etc. Covers requests like 'draw a cat', 'change the background to blue', 'generate a logo'. NOT for selfies — use the selfie skill for 'send a selfie', 'send me a selfie', 'take a selfie'.

### memory-management

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read, Write
- 路径：`/opt/Alma/resources/bundled-skills/memory-management`
- 说明：Search and manage Alma's memory and conversation history. Use when the user asks about past conversations, personal facts, preferences, or anything that requires recalling information ("do you know my...", "we talked about before...", "do you remember...", "help me find what we said about..."). Also used to store new memories and search through archived chat threads.

### music-gen

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/music-gen`
- 说明：Generate original songs with lyrics and vocals. Use when users ask to sing, create a song, make music, compose, or generate audio. Supports style prompts, custom lyrics, duration control, and instrumental mode.

### music-listener

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read
- 路径：`/opt/Alma/resources/bundled-skills/music-listener`
- 说明：Listen to and appreciate music files. Analyze audio for genre, mood, tempo, and lyrics. Use when users share audio/music files, ask about songs, or want music analysis.

### notebook

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read, Write
- 路径：`/opt/Alma/resources/bundled-skills/notebook`
- 说明：Edit Jupyter notebook (.ipynb) cells — insert, replace, or delete cells. Use when working with notebooks.

### plan-mode

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/plan-mode`
- 说明：Switch into structured planning mode before outlining multi-step solutions, and exit when done.

### reactions

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/reactions`
- 说明：React to a message with an emoji. Works on Telegram, Discord, and Feishu.

### scheduler

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read, Write
- 路径：`/opt/Alma/resources/bundled-skills/scheduler`
- 说明：Create, manage, and delete scheduled tasks (cron jobs) and configure heartbeat. Use when users ask for reminders, recurring tasks, daily summaries, periodic checks, or anything time-based. Also manages HEARTBEAT.md for periodic awareness checks.

### screenshot

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read
- 路径：`/opt/Alma/resources/bundled-skills/screenshot`
- 说明：Take screenshots of the screen using macOS screencapture. Use when users ask to see the screen, debug UI, or capture what's displayed. Resize before returning to avoid blowing up model context.

### self-management

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/self-management`
- 说明：Read and update Alma's own settings via the `alma` CLI. MUST USE when users ask to change voice, TTS, models, or any configuration. Run `alma voices` to list voices, `alma config list` to see all settings. ALWAYS use this skill instead of guessing.

### self-reflection

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read, Write
- 路径：`/opt/Alma/resources/bundled-skills/self-reflection`
- 说明：Daily self-reflection and personal growth. Triggered by heartbeat at end of day. Review the day's experiences, extract lessons, update personality, and write a diary entry.

### selfie

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read
- 路径：`/opt/Alma/resources/bundled-skills/selfie`
- 说明：Take selfies with consistent face/appearance. Use when users ask for selfies, self-portraits, or say things like 'send a selfie', 'take a selfie', 'snap one'. NOT for general image generation or editing — use image-gen for those.

### send-file

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/send-file`
- 说明：Send files, photos, audio, or videos to the current conversation (channel chat or GUI thread). MUST use whenever you need to deliver any file to the user. Covers: sending images, selfies, generated art, documents, music, videos, voice messages, screenshots, or ANY file the user asks to see. Triggers: 'send it to me', 'send it over', 'let me see', 'send me', 'show me', 'send photo', 'send file', sharing any file path. NEVER paste raw file paths in text — ALWAYS use this skill to send files.

### skill-hub

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read, Write
- 路径：`/opt/Alma/resources/bundled-skills/skill-hub`
- 说明：Search, install, and manage Alma skills from the skills.sh ecosystem. Use when the user needs a capability Alma doesn't have yet, or when you encounter a task you can't do with current skills.

### skill-search

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/skill-search`
- 说明：Search and install new skills to extend your capabilities. Use when you encounter a task beyond your current skills.

### system-info

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/system-info`
- 说明：Get system information — OS version, disk usage, memory, running processes, network status. Use when users ask about their computer status or system health.

### tasks

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/tasks`
- 说明：Global multi-step task tracking. Create, update, and monitor long-running tasks across threads. Tasks persist across restarts and are visible in all conversations.

### telegram

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/telegram`
- 说明：Interact with Telegram Bot API: send messages/photos/files/videos/audio/stickers/polls to any chat/group/channel, manage groups (pin/unpin/ban/kick/promote/restrict/set title/description/photo), forward/copy/delete/edit messages, react to messages, create invite links, manage forum topics, inline keyboards, and more. Use for ANY Telegram operation.

### thread-management

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/thread-management`
- 说明：Manage chat threads — create, list, switch, delete, and search conversations. Use when users want to organize their chats.

### todo

- 来源：bundled
- 启用：true
- 可用工具：Read, Write
- 路径：`/opt/Alma/resources/bundled-skills/todo`
- 说明：Manage a structured task list using a Markdown file in the workspace. Track progress on complex multi-step tasks. File-based — just Read and Write the todo file.

### travel

- 来源：bundled
- 启用：true
- 可用工具：Bash, Read, Write, WebSearch
- 路径：`/opt/Alma/resources/bundled-skills/travel`
- 说明：Virtual travel system. Explore real destinations, experience local culture, write travel diaries, and grow your personality through travel experiences. Manages departure, daily exploration, events, return, and personality evolution.

### twitter-media

- 来源：bundled
- 启用：true
- 可用工具：Bash, WebFetch
- 路径：`/opt/Alma/resources/bundled-skills/twitter-media`
- 说明：Extract content from Twitter/X links — text, images, videos, and thumbnails. Use when users share any twitter.com or x.com URL, or when you need to fetch media from a tweet. Triggers: any URL containing twitter.com or x.com, 'what did they post', 'what's in this tweet', 'show me that tweet'.

### video-reader

- 来源：bundled
- 启用：true
- 可用工具：未声明
- 路径：`/opt/Alma/resources/bundled-skills/video-reader`
- 说明：Read, watch, and listen to video/audio files. Use Gemini for native video understanding, or extract key frames + Whisper transcription as fallback. Use when a user sends a video/audio and asks about its content, what's in it, what someone said, etc.

### voice

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/voice`
- 说明：Generate voice messages using local Qwen3-TTS (offline, Apple Silicon). Convert text to speech with customizable voices, emotions, and speed. Use when user asks for voice reply, audio, or TTS.

### web-fetch

- 来源：bundled
- 启用：true
- 可用工具：Bash, WebFetch, ChromeRelayNavigate, ChromeRelayRead, ChromeRelayReadDom, ChromeRelayListTabs, ChromeRelayClick, ChromeRelayScreenshot, ChromeRelayScroll, ChromeRelayEval
- 路径：`/opt/Alma/resources/bundled-skills/web-fetch`
- 说明：Fetch and read web pages, APIs, and online content. Use when users share URLs or ask about web content.

### web-search

- 来源：bundled
- 启用：true
- 可用工具：Bash, WebSearch, WebFetch
- 路径：`/opt/Alma/resources/bundled-skills/web-search`
- 说明：Search the web for information using the built-in WebSearch tool. Use when users ask questions requiring up-to-date information, research, or fact-checking.

### xiaohongshu-cli

- 来源：bundled
- 启用：true
- 可用工具：Bash
- 路径：`/opt/Alma/resources/bundled-skills/xiaohongshu-cli`
- 说明：Use for ANY Xiaohongshu / 小红书 / Rednote / Little Red Book task: login, account status, search notes/users/topics, read note details/comments, browse feed and hot lists, like/favorite/comment/reply, follow/unfollow, check favorites/notifications/my-notes, post image notes, and delete your own notes.

## 4. Alma CLI 主要命令入口

从 `alma help` 可以看到当前 CLI 主要有这些方向：

- `alma status`
- `alma config list / alma config set`
- `alma providers`
- `alma voices`
- `alma threads`
- `alma skill list / alma skill search / alma skill install`
- `alma browser ...`
- `alma calendar ...`
- `alma tui`
- `alma run`

## 5. 目录里的技能导出位置

- 导出的 skill 仓库：`~/workspace/ai/alma-skills`
- bundled skills：`~/workspace/ai/alma-skills/bundled`
- custom skills：`~/workspace/ai/alma-skills/custom`

## 6. 说明

- Skill 是“能力包”，通常由 `SKILL.md` 加上可选脚本/资源组成。
- 一些 skill 强依赖 Alma 自己的工具名、CLI 命令、上下文约定。
- 如果要给别的项目复用，通常需要把 skill 里的调用方式适配成那个项目自己的工具层。
