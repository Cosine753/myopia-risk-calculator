# 儿童近视与干眼共病问卷风险分层计算器（公开研究演示版）

Questionnaire-based risk stratification of childhood myopia and myopia–dry eye
comorbidity — public research demonstration.

**在线地址：<https://cosine753.github.io/myopia-risk-calculator/>**
（2026-07-26 上线，GitHub Pages，公开可访问）

许可证 MIT · 模型版本 2026-07-22 · 引用版本 `v1.0.0`

> **仍未做的事**
> - **没有持久标识符（DOI）**。已加许可证并打了 `v1.0.0` 标签，但期刊的 Code
>   availability 通常还要求一个不会变的存档标识符。见下文「引用与版本固定」。
> - **未公开署名**。`CITATION.cff` 尚未提交，页面页脚也没有作者／单位；模板放在
>   `_local/CITATION.cff.template`，等稿件里的 `[AUTHOR TO COMPLETE]` 定稿后再填。
>   `LICENSE` 的版权行现在写的是 GitHub 账号名 `Cosine753`，届时要一起换成真实
>   版权主体。
> - 注意署名与 DOI 有先后顺序：Zenodo 会读取 `CITATION.cff` 生成元数据，**先铸
>   DOI 会把作者登记成 GitHub 账号名**。

## 这是什么

论文中锁定的两阶段 Logistic 模型的浏览器演示版。用户填写 15 项非侵入性问卷条目
（Stage 1 的 10 项与 Stage 2 的 8 项之并集，`Age`、`Gender`、`Daily_Study_Time`
同时进入两个阶段），页面输出：

- 初始模型概率 `pA`（近视相关状态 vs 健康），阈值 0.575；
- 条件模型概率 `pC|A`（近视–干眼共病 vs 单纯近视），阈值 0.150；
- 三分类组合概率 `1−pA`、`pA×(1−pC|A)`、`pA×pC|A`，以及顺序阈值规则给出的风险分层。

模型版本 2026-07-22。页面不会重新训练模型、更新系数或调整阈值。

## 与上游 Next.js 版的关系

**本仓库是论文引用的版本**，也是这条锁定路径当前唯一在线的实现。

`D:\.ayanke\Submission_Package_Final\07_Web_Demo\Public_Online_Version` 里的
Next.js + Cloudflare Worker 构建是**早期私有部署的存档**，2026-07-26 起已被取代，
只作溯源保留，不再是改动的起点。本仓库最初是那一版的手工静态移植：页面结构、中文
文案、配色与响应式断点照搬其 `app/page.tsx` 与 `app/globals.css`，上游由 Tailwind
preflight 提供的基础 reset 在本版中以等价的手写 reset 替代。

因此**不需要**、也不应该再把上游的改动往这边搬。上游那份代码已冻结；`Public_Online_Version/tests/`
下的测试覆盖的是那个应用，不是本页面。两边不存在自动同步机制，这是有意为之。

## 公开安全边界

- **不包含任何参与者记录**：201 例机构留出验证数据没有进入本仓库，页面内也不含
  任何个体层面的数据。
- **无后端**：没有数据库、账号系统、表单提交接口、分析统计或第三方脚本。
- **无外部依赖**：整站为单个 `index.html`，无 CDN、无 Google Fonts、无网络请求，
  中国大陆网络环境下可完整加载。
- **不写入浏览器存储**：问卷选项仅参与当前页面的一次计算，不使用 cookie 或
  localStorage。
- **不是诊断工具**：结果仅用于问卷风险分层和科研展示，不能替代眼科检查、临床诊断
  或治疗建议；未经前瞻性或外部验证，且使用的固定阈值在论文自身的阈值稳定性分析中
  已被证明不可迁移。

## 核验

分析单位为一例机构留出验证记录（n = 201）。核验脚本
`_local/verify_deployed_build.ps1`（因引用非公开数据路径而不随仓库发布）执行四项检查：

| 检查 | 2026-07-26 结果 |
|---|---|
| 线上页面与仓库文件一致（换行归一化后逐字节） | 一致 |
| 内嵌模型 vs 归档锁定 JSON：截距、2 个阈值、两个变量表、全部 51 个类别系数 | 全部相同 |
| 201 例留出记录重算 vs 归档锁定输出 | 最大绝对偏差 `3.331e-16`；三分类概率和偏离 1 最大 `2.220e-16`；分层 56/77/68；0 例分歧 |
| 页面自包含性：外链、`@font-face`、CSS `url()`、`fetch`/XHR、表单 `action`、浏览器存储 | 均无 |

线上文件 SHA-256（LF）：
`DFF5204B9B9483880B2170B4CA1AA0015BC0CDF1CFDDF572FFD8089225C1A70C`（46,404 字节）。

这些都是**实现层面的一致性检查**，只证明页面忠实复现了锁定路径，不提供任何关于模型
性能的新证据。核验记录见非公开材料包 `00_QA/Public_Online_Calculator_20260726.md`。

## 引用与版本固定

`main` 是活动分支，随时可能变化，因此**不要用裸 URL 作为论文的 Code availability**。

- 本次核验对应的不可变版本：标签 `v1.0.0`
- 该标签的固定快照：<https://github.com/Cosine753/myopia-risk-calculator/tree/v1.0.0>

本仓库的公开提交历史起自 `v1.0.0`（2026-07-26）。此前的开发历史只保留在本地非公开
材料中，因此非公开 QA 记录里引用的早期 commit SHA 在本仓库中无法解析——这是有意为
之，不是仓库损坏。`index.html` 的内容与上表的核验结果不受影响。

还需要一个持久标识符。两条路，选其一或都做：

1. **Zenodo DOI（推荐，需要账号操作）**：登录 <https://zenodo.org> → 用 GitHub 账号
   授权 → 在 GitHub repositories 列表里为本仓库打开开关 → 回到 GitHub 发一个
   Release → Zenodo 自动归档并铸 DOI。

   顺序很重要：**先把 `CITATION.cff` 填好提交，然后从包含它的提交打一个新标签
   （例如 `v1.0.1`），再从那个标签发 Release**。Zenodo 只在 Release 时抓取快照，
   `v1.0.0` 里没有 `CITATION.cff`，从它发 Release 会把作者登记成 GitHub 账号名。

   这样做不会影响已有核验：`v1.0.1` 相对 `v1.0.0` 只多一个 `CITATION.cff`，
   `index.html` 一个字节都不变，上面那张核验表继续成立。DOI 铸好后把它写回
   `CITATION.cff` 的 `doi:` 字段和稿件的 Code availability 段。
2. **Software Heritage（无需账号，但会主动把仓库提交给外部存档）**：在
   <https://archive.softwareheritage.org/save/> 提交仓库 URL，得到一个 SWHID 永久
   标识。此操作对外公开，执行前请自行确认时机。

## 文件

| 文件 | 说明 |
|---|---|
| `index.html` | 全站唯一页面，样式、逻辑与锁定模型参数均已内嵌 |
| `LICENSE` | MIT 许可证（版权行待替换为真实版权主体） |
| `.nojekyll` | 关闭 GitHub Pages 的 Jekyll 处理 |
| `_local/` | 不发布。核验脚本与 `CITATION.cff` 模板，引用非公开数据路径 |

## 本地预览

直接用浏览器打开 `index.html` 即可，无需构建工具或本地服务器。

## 部署（已完成，此处仅供日后维护）

站点已部署在 GitHub Pages：仓库 `Cosine753/myopia-risk-calculator`，
Source = Deploy from a branch，Branch = `main` / `(root)`。

之后每次修改内容，只需在本目录执行：

```
git add . && git commit -m "更新" && git push
```

推送后约 1 分钟自动重新发布，无需再动 Pages 设置。

## 修改 `index.html` 后必须做的事

页面已被论文引用，且核验记录里写有线上文件的 SHA-256，因此**任何对 `index.html`
的改动都不是免费的**（连纯样式或无障碍性微调也不例外）。改完要按顺序：

1. 重新执行 `_local/verify_deployed_build.ps1`，必须 PASS；
2. 更新 `00_QA/Public_Online_Calculator_20260726.md` 里的 SHA-256 与核验日期；
3. 同步刷新归档副本 `07_Web_Demo/GitHub_Pages_Version/`（该目录存 CRLF，哈希与线上
   LF 版本按设计不同）；
4. 打新标签，并检查稿件里引用的版本号是否还对得上。

模型参数以 JSON 形式内嵌在 `index.html` 末尾的
`<script type="application/json" id="model-data">` 块中。若要更新系数或阈值，必须同步
替换该块并重新执行核验，不要只改页面上显示的阈值文字。
