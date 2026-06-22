# AI Quant CLI — 财报 → HTML 投研报告（第 18 节配套）

把一份 A 股年报 PDF，自动解析三张合并报表 → 由 **Claude Code 亲自**做财务风险研判与三表勾稽 → 出图 → 生成一份 HTML 投研报告。

> 设计内核：**财务分析判断由 Claude Code 直接做，代码不调用大模型 API**；Python 脚本只做确定性工作（解析 PDF、出图、汇编 HTML）。详见 `CLAUDE.md`。

## 学习要点（本节你该掌握什么）
- **先设计后施工**：从零造系统，先让 Claude Code 出架构蓝图、你 review，再逐层施工——别一上来就写代码。
- **智能在 Claude Code 的循环里，不在 API 调用里**：复杂判断（财务研判）让它亲自做、可追问可解释，而不是写脚本调大模型 API。
- **按需检索**：两百多页年报只取需要的三张合并报表，不通读、不全塞。
- **自愈**：给目标不给步骤，让它在报错（缺包 / 表格定位 / 中文乱码…）里自己查、自己修。
- **三表勾稽**：用资产负债表 / 利润表 / 现金流量表互相印证，发现单看一张表看不出的风险。
- **自愈 + 记忆**：把踩坑的修法写回 `CLAUDE.md`，系统越用越顺。

## 复课：两种用法

### A. 跑现成的（复现 / 研究代码）
```bash
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple   # 国内源更快
# 下载年报 PDF 放进 data/（见 data/README.md，巨潮直链）
python scripts/run_pipeline.py        # 一键端到端
```
报告出在 `reports/`，文件名 `report_<股票代码>_<报告期>_<时间戳>.html`——**多次跑、多家公司都不覆盖**，历史留痕。

### B. 从零自己造一遍（进阶，推荐——这就是课上做的）
新开一个**空目录**，把本仓库的 `BUILD_PLAN.md` 和 `data/`（放好 PDF）拷进去，启动 Claude Code（**Auto 模式**），发一句：
> 读 BUILD_PLAN.md，按里面的阶段一步步把这个系统做出来。每完成一个阶段就停下，等我说「继续」。

自己建完，再拿本仓库 `src/` 当参考实现对比。课程演示的就是这条路径。

## 结构
| 路径 | 是什么 | 入库 |
|---|---|---|
| `src/ai_quant/` | 核心代码：parsing 解析 / viz 出图 / report 汇编 / pipeline 编排 | ✓ |
| `scripts/` | 各步入口 + `run_pipeline.py` 一键端到端 | ✓ |
| `CLAUDE.md` / `BUILD_PLAN.md` | 项目记忆与设计约定 / 从零构建计划 | ✓ |
| `analysis/findings_<代码>.json` | 各标的的研判结果（参考） | ✓ |
| `data/parsed/financials.json` | 解析出的结构化数据样例 | ✓ |
| `data/*.pdf` | 年报原件 | ✗ 自行下载（版权） |
| `reports/` `build/` | 运行产物（报告网页、图表） | ✗ 跑一次就有 |

## 数据
年报 PDF 不入库。从巨潮资讯网（cninfo.com.cn）按股票代码搜「年度报告」下载，放进 `data/`（详见 `data/README.md`）。
