# 📖 在线阅读 · ChatGPT-MCP 插件制作教程

> 《内部系统接入 ChatGPT：MCP + OAuth 只读插件制作教程》——从零到一，把任何没有公开 API 的内部业务系统，安全地变成 ChatGPT 可用的只读工具。全文脱敏 · 可公开分享 · 实战验证通过。以下为原 PDF 逐页呈现。如需下载原版：[ChatGPT-MCP插件制作教程_脱敏版.pdf](./ChatGPT-MCP插件制作教程_脱敏版.pdf)

**快速跳转**：[目标 · 架构 · 清单](#目标--架构--准备清单) · [第一步 封装 MCP 服务](#第一步--封装-mcp-服务) · [第二步 Auth0 OAuth 授权](#第二步--auth0-搭建-oauth-授权服务) · [第三步 服务端接入与公网暴露](#第三步--服务端-oauth-接入与安全暴露公网) · [第五步 ChatGPT 创建连接](#第五步--在-chatgpt-创建连接) · [踩坑实录 · 安全验收](#踩坑实录--安全验收清单) · [一页纸 Checklist](#一页纸复用-checklist)

---

## 目标 · 架构 · 准备清单

> 要做出什么效果、整体架构（MCP Server + Caddy + Cloudflare Tunnel + Auth0）、准备清单与费用。

![目标 架构 准备清单](pages/page-1.png)

## 第一步 · 封装 MCP 服务

> 把没有公开 API 的内部系统封装成 MCP 服务。

![第一步 封装 MCP 服务](pages/page-2.png)

## 第二步 · Auth0 搭建 OAuth 授权服务

> 签发 JWT 令牌（RS256），ChatGPT 拿令牌访问 MCP 服务。

![第二步 Auth0 OAuth 授权服务](pages/page-3.png)

## 第三步 · 服务端 OAuth 接入与安全暴露公网

> 令牌校验（无令牌一律 401）+ Cloudflare Tunnel 与 Caddy 分流，对内走 localhost、对外强制 OAuth。

![第三步 服务端接入与公网暴露](pages/page-4.png)

## 第五步 · 在 ChatGPT 创建连接

> 开启开发者模式、创建应用、OAuth + DCR 授权与工具清单核对。

![第五步 在 ChatGPT 创建连接](pages/page-5.png)

## 踩坑实录 · 安全验收清单

> 实战中踩过的三个坑（DCR 开关、域级连接、Google 登录）排错实录 + 上线后逐条过的安全验收清单。

![踩坑实录与安全验收](pages/page-6.png)

## 一页纸复用 Checklist

> 换一个内部系统复用整套方案的一页纸流程。

![一页纸复用 Checklist](pages/page-7.png)

---

## 关于作者

**作者：simon，一个不懂代码的 AI 极客**

- 微信交流群：`simonsxxs`
- 个人网站：https://simonsxx.com
- GitHub：https://github.com/simonsxxs
- Gitee：https://gitee.com/simonsxx
