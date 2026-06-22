# data/ — 放年报 PDF（不入库）

把宁德时代 FY2025 年报 PDF 放在本目录，再启动构建（见上级 `BUILD_PLAN.md`）。

**下载**
- 巨潮资讯网 → 搜 `300750` → 公告 → 年度报告 → 「2025年年度报告」
- 直链：`http://static.cninfo.com.cn/finalpage/2026-03-10/1225002214.PDF`

> A 股年报 PDF **不入库**（版权稳妥）；解析后的结构化样例数据可以入库。
> 构建阶段 1 里 Claude Code 会生成 `.gitignore` 排除本目录下的 PDF / 数据。
