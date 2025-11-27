投身 vibe coding 已经有一段时间，上手的项目有上百个功能点的商业 SaaS，也有自己用的 iOS 小 app，目前开发环境已经差不多稳定下来，更好可以做个总结。

过程中不断感慨，现在的软件开发已经彻底不同以往，以一当十的时代已经到来。虽然愚蠢、虽然受限，但结果总体上真的是又快又好。

## 概览
开发环境： Claude Code 为主，Gemini CLI 辅助
模型：
- Sonnet 4.5 + Opus 4.5
- 偶尔用 GLM 4.6/Kimi K2  Thinking 快速处理一些小任务。
- 同时也会用 Gemini

## 节奏感
要有好的节奏感，需要：
- 提升效率——使用语音输入法（豆包）
- 减少等待——建立并发
- 减少卡壳——终端用 warp，不用搜命令
- 实时提醒——善用 Claude Code hooks

我的 `~/.claude/settings.json`
```json
{
  "hooks": {
    "Notification": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "afplay /System/Library/Sounds/Submarine.aiff;terminal-notifier -message '需要您的输入确认...' -title 'Claude Code' -sender com.apple.Terminal"
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "afplay /System/Library/Sounds/Funk.aiff;terminal-notifier -message 'Claude Code 任务完成' -title 'Claude Code' -sender com.apple.Terminal"
          }
        ]
      }
    ]
  },
  "editor": {
    "autoApply": false,
    "confirmBeforeEdit": true
  },
  "model": "opus"
}

```



## 上下文
上下文基本上是有「各地的」`CLAUDE.md` ，配合其它 markdown 文档来实现。
有哪些地方呢？
- `~/.claude/CLAUDE.md` 整个电脑的全局规则。
-  `项目根目录/CLAUDE.md`  项目的全局规则。
- `项目根目录/docs/` 存储项目的各种 markdown 文档， 建议单独建一个文档的 repo（如 foobar_docs），避免日常工作文档和项目部署混杂在一起。这个目录可以用 Obsidian 管理，使用 git 插件更新。


`项目根目录/CLAUDE.md`里会索引「必读文档」：

```markdown
**必读文档**（按优先级）：
1. **`/Users/watchelephant/www/jmt/docs/跨端开发最佳实践.md`** - 日常开发必须遵守的规范
2. **`/Users/watchelephant/www/jmt/docs/架构方案.md`** - 项目整体架构设计
3. **`/Users/watchelephant/www/jmt/docs/15天开发计划.md`** - 15 天开发计划和执行指南
4. **`/Users/watchelephant/www/jmt/docs/TODO.md`** - 任务追踪和进度管理
5. **本文件** - 开发约束和环境配置
   
文档路径速查：
需求文档：  /Users/watchelephant/www/jmt/docs/PRD.md          # 完整产品需求
任务追踪：  /Users/watchelephant/www/jmt/docs/TODO.md         # 按 Day 分解的任务清单
当前任务：  /Users/watchelephant/www/jmt/docs/CURRENT.md      # 当天聚焦任务（AI 生成）
开发计划：  /Users/watchelephant/www/jmt/docs/15天开发计划.md
架构方案：  /Users/watchelephant/www/jmt/docs/架构方案.md
跨端规范：  /Users/watchelephant/www/jmt/docs/跨端开发最佳实践.md
原型文件：  /Users/watchelephant/www/jmt/prototype/

### 上下文管理策略

为降低 Token 消耗，采用**聚焦文件**策略：

- **PRD.md** (831行) - 完整需求，仅在需要时读取特定章节
- **TODO.md** (797行) - 完整任务清单，用于同步进度
- **CURRENT.md** - 当天任务聚焦文件（<100行），日常开发主要读取此文件
```

## CLAUDE.md 里要有什么
1. 代码目录的导航：目录树结构、路径速查表
2. 对话的规范：什么时候应该记录新规范
3. 技术栈和开发风格/基础约定
4. 测试数据/测试账号的生成约定
5. 一些希望它自动化的东西：比如自动生成每日总结/更新说明的逻辑、每日启动 TODO 要做的事情
6. 其它文档的导航：PRD、TODO、技术架构等等

随着开发的进行，`CLAUDE.md` 里记的东西会越来越多，其实就跟人类平时的笔记一样，不必强求结构化和干净，但也注意定时整理、避免上下文过长就好。


## 模型接力
模型经常需要换来换去，我是用 shell 脚本解决模型环境切换的问题。
CC 镜像1
```
# Linux/macOS 启动 aiClaude.sh
unset ANTHROPIC_AUTH_TOKEN
unset ANTHROPIC_API_KEY
unset ANTHROPIC_BASE_URL
unset ANTHROPIC_MODEL
unset ANTHROPIC_SMALL_FAST_MODEL
export ANTHROPIC_BASE_URL=https://api.aicoding.sh
export ANTHROPIC_AUTH_TOKEN=aicoding-xxxx
export ANTHROPIC_MODEL=
export ANTHROPIC_SMALL_FAST_MODEL=
# 拉长客户端等待，避免上游超时导致前端先断
export API_TIMEOUT_MS=600000
# 关闭非必要流量（遥测/错误上报/bug 命令/自动更新）
export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
claude

```
CC 镜像 2
```
# Linux/macOS 启动 ccClaude.sh
unset ANTHROPIC_AUTH_TOKEN
unset ANTHROPIC_API_KEY
unset ANTHROPIC_BASE_URL
unset ANTHROPIC_MODEL
unset ANTHROPIC_SMALL_FAST_MODEL
export ANTHROPIC_BASE_URL=https://api.claudecode-cn.com/api/claudecode
export ANTHROPIC_API_KEY=xxxxx
export ANTHROPIC_MODEL=
export ANTHROPIC_SMALL_FAST_MODEL=
# 拉长客户端等待，避免上游超时导致前端先断
export API_TIMEOUT_MS=600000
# 关闭非必要流量（遥测/错误上报/bug 命令/自动更新）
export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
claude

```

GLM 
```
# Linux/macOS 启动 glmClaude.sh
unset ANTHROPIC_AUTH_TOKEN
unset ANTHROPIC_API_KEY
unset ANTHROPIC_BASE_URL
unset ANTHROPIC_MODEL
unset ANTHROPIC_SMALL_FAST_MODEL
export ANTHROPIC_BASE_URL=https://open.bigmodel.cn/api/anthropic
export ANTHROPIC_AUTH_TOKEN=xxxx
export API_TIMEOUT_MS=3000000
export ANTHROPIC_MODEL=glm-4.6
export ANTHROPIC_SMALL_FAST_MODEL=glm-4.6
export ANTHROPIC_DEFAULT_OPUS_MODEL=glm-4.6
export ANTHROPIC_DEFAULT_SONNET_MODEL=glm-4.6
export ANTHROPIC_DEFAULT_HAIKU_MODEL=glm-4.5-air
claude
```

Kimi
```
# Linux/macOS 启动 kimiClaude.sh
# 说明 https://platform.moonshot.cn/docs/guide/agent-support#%E4%BD%BF%E7%94%A8%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9
unset ANTHROPIC_AUTH_TOKEN
unset ANTHROPIC_API_KEY
unset ANTHROPIC_BASE_URL
unset ANTHROPIC_MODEL
unset ANTHROPIC_SMALL_FAST_MODEL
export ANTHROPIC_BASE_URL=https://api.kimi.com/coding/
export ANTHROPIC_API_KEY=sk-kimi-xxxx
export ANTHROPIC_MODEL=kimi-k2-thinking-turbo
export ANTHROPIC_SMALL_FAST_MODEL=kimi-k2-turbo-preview
claude

```