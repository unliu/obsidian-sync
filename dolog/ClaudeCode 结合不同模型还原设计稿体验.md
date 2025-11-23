
## 总结
- 优先还是官方镜像
- 次选 Kimi，主模型 0905，快速模型 turbo
- 再次选 longcat（但免费 token 只有 10w，给的很抠）

## 过程
选区
![[Pasted image 20250907125000.png]]
配置方式
```sh
# mirrorClaude.sh
# https://www.claudecode-cn.com/dashboard/official-installation/macos-linux
export ANTHROPIC_BASE_URL=https://api.claudecode-cn.com/api/claudecode
export ANTHROPIC_API_KEY=sk-ant-api03-lUTzqdW-UkumJi5sJMTODt_Bu8HrVHLC0PmyrttwKjacOawoEc8kynPHc8xz0f-Z-H7IVU40kJH1MYQHj3_5uQ
claude

> https://www.figma.com/design/ZNTUSurph0hpgZAerYFgy8/HelloWorld?node-id=27-2528&t=32MMePHHGKYSSNZx-4 还原这个设计到 test_mClaude.html
> 请以1600乘以960的一个卡片展示在屏幕的正中间。
```
唯一比较完美实现，未需要反复提示的；而且又快又好：
![[Pasted image 20250907125630.png]]


```
# kimiClaude.sh
# Linux/macOS 启动高速版 kimi-k2-turbo-preview 模型
export ANTHROPIC_BASE_URL=https://api.moonshot.cn/anthropic
export ANTHROPIC_AUTH_TOKEN=sk-vS8ciJTB5Vk8wiYkFIKxdn7xaZMmuZWAhvDjKyXNQw7GGbUU
export ANTHROPIC_MODEL=kimi-k2-0905-preview
export ANTHROPIC_SMALL_FAST_MODEL=kimi-k2-turbo-preview
claude

# 一开始都用 turbo
```
主模型用 turbo 版本时：
- 连接 MCP 多次失败后，重启 figma 成功（可能不是 kimi 的问题）
- 粘贴图片，会导致进程死循环
- 重启后可用，但图片抓不下来，多次追问声称抓了但实际只有一部分
- ![[Pasted image 20250907131047.png]]
- ![[Pasted image 20250907131449.png]]
主模型用 0905 版本时：
- 一次成功无反复，但效果一般
![[Pasted image 20250907131252.png]]
![[Pasted image 20250907131408.png]]

```
# qwenClaude.sh
# https://help.aliyun.com/zh/model-studio/claude-code
export ANTHROPIC_BASE_URL=https://dashscope.aliyuncs.com/api/v2/apps/claude-code-proxy
export ANTHROPIC_AUTH_TOKEN=sk-a4134d0f275e44e7ba0776c67e87c181
claude
```
![[Pasted image 20250907125751.png]]
没有太多反复，疑似仅网络问题，但效果一般，时间 7 倍：
![[Pasted image 20250907125836.png]]


```

# glmClaude.sh
# https://docs.bigmodel.cn/cn/guide/develop/claude 不好用 405 报错；口碑：贼慢，有点傻
export ANTHROPIC_BASE_URL=https://open.bigmodel.cn/anthropic
export ANTHROPIC_AUTH_TOKEN=58df5bb0e4c94a48b6193ec8da2e8780.BXIBWfliuHQACRXU
export ANTHROPIC_MODEL=glm-4.5
export ANTHROPIC_SMALL_FAST_MODEL=glm-4.5-air
claude

```
根本就连不上服务器（一直 API Error: 405）

```
# longcatClaude.sh
# https://longcat.chat/platform/docs/#anthropic-api-format
export ANTHROPIC_BASE_URL=https://api.longcat.chat/anthropic
export ANTHROPIC_AUTH_TOKEN=ak_1qC4jx8Xk3dA6Tw5U279m1sq8y62U
export ANTHROPIC_MODEL=LongCat-Flash-Chat
claude
```
一开始生成的完全空白，然后布局完全乱掉，然后文字背景没有半透明，多次尝试
但布局理解基本正确，有耐心，最终效果接近原生 Claude
![[Pasted image 20250907132826.png]]
![[Pasted image 20250907131544.png]]

```
# deepClaude.sh
# https://api-docs.deepseek.com/zh-cn/guides/anthropic_api
export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
export ANTHROPIC_AUTH_TOKEN=sk-da7105f8d9a94d1aa887b3eedeb95618
export API_TIMEOUT_MS=600000
export ANTHROPIC_MODEL=deepseek-chat
export ANTHROPIC_SMALL_FAST_MODEL=deepseek-chat
export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
claude

```
始终不成功：
![[Pasted image 20250907130118.png]]