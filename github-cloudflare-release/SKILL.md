---
name: github-cloudflare-release
description: 将完成的网站项目保存到 GitHub，并在审查免费额度、兼容性和收费风险后，经用户决定连接 Cloudflare 自动部署。用于“先上传 GitHub 再上线”“免费部署检查”及该流程的后续更新。
---

# GitHub 保存与 Cloudflare 发布

## 用户选择

项目先保存到用户的 GitHub；在部署 Cloudflare 前报告适用的免费额度、平台限制及外部服务费用。遇到任何未获用户接受的有限额度、兼容性限制、收费条件或未知项，停在 GitHub，等用户决定。不要将“有免费套餐”解释为“无限免费”。普通静态托管也存在文件、构建等限制，首次发布必须说明。

这是按需执行的工作流技能，不是后台监控器，也不是自动费用封顶工具。仅在用户请求保存、发布或更新项目时执行。创建本技能本身不授权迁移当前项目、创建仓库或部署现有网站。

## 1. 确认项目与既有部署

- 读取适用的 AGENTS.md；按需定位源码、构建命令、发布目录、Git remote、当前分支、Cloudflare 配置及现有部署记录。复用现有项目，不因目录中有文件就新建站点。
- 发现并优先使用已连接的 GitHub/Cloudflare 工具；缺少能力时再用官方 CLI 或浏览器。不要索取明文密码、API 密钥或持久化凭据。
- 默认每个独立产品一个仓库；复用用户指定的仓库。新仓库默认私有，保留既有可见性；公开网站不自动要求公开源码。
- Cloudflare Pages、Workers、数据库、文件存储及第三方接口分别评估，不套用另一产品的免费规则。不因 Sites 插件存在就把用户指定的 Cloudflare 发布改成 Sites 发布。

## 2. 保存 GitHub 前识别自动发布副作用

先检查 Cloudflare 的生产分支、预览分支规则、GitHub Actions，以及其他会发布或产生费用的 webhook/workflow。检查不是只看本地配置；无法读取远程规则就报告未知。

- 新建仓库尚未连接部署时，可以先保存 GitHub，再做部署审查。
- 仓库已经连接自动部署时，提交到生产分支可能立即上线；非生产分支也可能自动生成付费/受限预览。未经审查通过，不推送到会触发这些行为的分支。
- 如需“只存 GitHub”，使用已确认不触发发布/预览的分支。无法确认安全保存分支时，先准备本地提交并解释阻碍，等用户选择；不得擅自关闭现有工作流或声称代码已经上传。
- 检查提交范围，排除凭据、`.env` 实值、用户个人数据、缓存和可重建依赖；可提供无实值的 `.env.example`。大体积用户文件和数据库内容不作为普通源码上传。
- 完成与项目有关的必要检查、构建或预览后提交并推送。记录仓库 URL、分支和完整 commit SHA。

## 3. 发布前审查

根据代码和构建产物判断实际架构，不能仅凭框架名字或“有动画”分类：

- 浏览器端：CSS 动画、GSAP、Three.js/WebGL、Canvas、浏览器计算及本地存储本身不产生服务器函数调用。
- 静态请求：HTML、JS、CSS、图片、字体、3D 模型、预生成 JSON 等；检查体积、数量、外链服务及兼容性。
- 服务器端：Functions、Worker 入口、API 路由、SSR、认证、代理、服务器图像转换、WebSocket/协作、定时任务等。
- 检查 middleware、`_worker.js`、`functions/`、`_routes.json` 和框架生成的路由。静态图片请求也可能先经过鉴权/转换函数；不能按文件扩展名推断请求免费。
- 存储/外部服务：数据库、对象存储、邮件、地图、支付、AI、图片和视频处理等，分别列出供应商与计费路径。构建时调用 AI/API 同样可能收费。

从当前官方资料验证实际使用产品的价格、限额、免费超额行为及项目是否需要计费账号。不要永久写死数字，也不要用固定未来流量保证免费。缺少流量预期时，明确免费边界；涉及按量运行服务时询问足以影响决策的预期用量，不以零流量代替估计。

在继续前给用户一份简短的具体审查结果：

| 项目 | 实际用途/预计用量 | 已核对的免费额度或限制 | 超额结果 | 是否需要用户决定 |
| --- | --- | --- | --- | --- |

报告必须说明：浏览器承担哪些计算、Cloudflare 承担哪些计算和存储、适用文件/构建限制、外部费用、现有付费套餐是否可能产生增量费用，以及没有查明的事项。附对应官方来源与核对日期。

## 4. 决策门槛

出现未获接受的额度或限制，即使目前预计不超额，也先完成可安全进行的 GitHub 保存，再询问是否在已说明的边界内启用 Cloudflare；没有回答不等于同意。不默认接受免费试用、绑卡、套餐升级或第三方付费服务。

用户在当前项目已明确批准过相同产品、限制、费用范围和公开对象，且本次变更仍在该范围内时，可沿用批准，不重复确认。新增服务器能力、付费依赖、未知用量或改变原边界时，重新说明并等待决定。

用户拒绝或尚未决定：保留 GitHub 源码与必要本地部署准备，停止 Cloudflare 创建/连接/部署操作，报告“已保存 GitHub；未部署”。不要悄悄部署到其他平台。

## 5. 用户批准后配置 Git 自动部署

- 首次配置推荐：普通开发提交放 `main`，只让专用 `production` 分支触发正式发布，关闭开发分支自动预览。先取得用户对这些新分支/部署规则的授权；已有项目保留既有规则，除非用户同意修改。
- 保存代码不等于批准发布。审查通过后，把已验证版本推送/合并到批准的发布分支，由 Cloudflare 自动部署。
- 明确自动部署系统本身不会阅读本技能或审查费用：手动推送发布分支、第三方提交仍可能触发上线。不要承诺技能能替代服务端预算控制或持续监控。
- 连接 GitHub 时仅授权选定仓库；遇到平台授权、登录、协议等要求，按实际工具权限流程处理。
- 使用免费提供的子域名；保留用户已选域名。静态工具不主动增加登录或数据库。
- Pages Direct Upload 不能原地切换为原生 Git 集成。若是现有 Direct Upload 项目，先说明迁移方案、项目/网址是否改变，获得同意再建立新 Git 集成项目；不要删除旧项目或承诺旧子域名必能转移。
- 构建命令、发布目录和运行时按实际项目填写；GitHub 保存完整必要源码，Cloudflare 发布构建产物。不要把服务端密钥写进前端代码。

## 6. 验证与交付

等待部署进入成功或失败终态。部署成功后，以普通未登录访客验证入口、静态资源和关键交互；受限后台按授权测试。报告未完成的验证，不能把“已上传/已排队”称为上线成功。

在项目内保留精简发布记录：仓库、分支、commit SHA、Cloudflare 项目/实际 URL、构建命令/输出目录、自动发布规则、批准的费用范围/限额及核对日期。不要保存令牌或私人账号数据。

最终只说明 GitHub 保存结果、部署或未部署状态、验证结果，以及需用户处理的限制。不要因为审查不通过而遗漏已授权且可安全完成的 GitHub 保存。

## 官方资料入口

只读取本项目实际涉及的页面并验证当前内容：

- Pages Git 集成：https://developers.cloudflare.com/pages/configuration/git-integration/github-integration/
- Pages 分支规则：https://developers.cloudflare.com/pages/configuration/branch-build-controls/
- Pages 限制：https://developers.cloudflare.com/pages/platform/limits/
- Direct Upload 与迁移边界：https://developers.cloudflare.com/pages/get-started/direct-upload/
- Functions 路由：https://developers.cloudflare.com/pages/functions/routing/
- Pages/Functions 价格：https://developers.cloudflare.com/pages/functions/pricing/
- Workers 价格：https://developers.cloudflare.com/workers/platform/pricing/
- D1 价格：https://developers.cloudflare.com/d1/platform/pricing/
- R2 价格：https://developers.cloudflare.com/r2/pricing/
- Workers AI 价格：https://developers.cloudflare.com/workers-ai/platform/pricing/
