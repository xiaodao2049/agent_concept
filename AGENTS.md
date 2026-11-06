# AGENTS.md

给接手本仓库的 AI 代理的操作说明。人类读者请直接看 [大模型领域概念全景.md](大模型领域概念全景.md)。

## 仓库定位

- 一份面向工程师的概念讲解文档，主题是 Model / Harness / Tool / MCP / Skill / Plugin / RAG / Memory / Subagent 的分层、协作与辨析。
- 产物只有两类：Markdown 正文 + SVG 示意图（可编辑 HTML 源 + 2 倍 PNG）。
- 没有构建系统、没有示例工程；克隆后直接阅读或打开 `diagrams/*.html` 即可。
- 讲的是会随时间变化的事实（协议版本、模型代际、Qoder 能力清单）。改事实时同步改 §8 时间线与 §7 速查表，不要只改一处。

## 目录

```text
AGENTS.md                 本文件
大模型领域概念全景.md      正文，图 1–7 均在此引用
diagrams/
  <name>.html             示意图源文件，内联 SVG，浏览器可直接打开
  <name>.png              同尺寸 2 倍导出，供 Markdown 引用与离线查看
```

## 改文档

1. 新概念先落位：归入 §1 的六层坐标系，再在 §2 增加词条，最后补 §7 速查表。缺少任何一处都算未完成。
2. 流程、时序、往返、循环类内容优先画图，不要写成 ASCII 时序图或编号步骤清单。
3. 结构性内容保留文本块，不要为了统一而改成图。当前有意保留的三处是：§1 六层坐标系、§2.4 Skill 目录树、§2.7 MCP/A2A 对应关系。
4. 图号连续（图 1–7），引用处的图注格式固定为：

   ```markdown
   ![图 N：一句话标题](diagrams/<name>.png)

   > **图 N｜一句话结论。** 读图说明……可编辑源文件：[<name>.html](diagrams/<name>.html)
   ```

5. 图文不要重复：图注只补充“怎么看这张图”，不重抄正文段落。图与正文之间、图注后与下一段之间，各留一个空行。

## 改图

- 单图规范来源是 diagram-design 技能（默认皮肤）：`paper #f5f5f5`、`ink #2d3142`、`muted #4f5d75`、`accent #bf4520`；标题 Instrument Serif，节点名 Geist 600，副标签/箭头标签 Geist Mono。
- 硬约束：≤ 9 个节点、≤ 12 条箭头、accent 只用在 1–2 个焦点元素；结构尺寸落在 4px 网格。
- 连接线只用正交折线（拐角 `r=8`，紧凑处最低 `r=6`），禁止斜线；箭头标签必须压在不透明遮罩上，与线保持 6–10px 间距，最多 14 个字符；并行路线间距 ≥ 12px。
- 每个 SVG 保留无障碍契约：根节点 `role="img"`、`aria-labelledby`、首个子元素 `<title>`、`<desc>`，可引用 id 加文件前缀，避免多图内联时串味。
- 改完 HTML 必须重出 PNG；PNG 不入版本管理的唯一例外是它本身是交付物，必须与 HTML 同批更新。

## 导出与自检

技能目录带版本号，更新插件后会变，以实际目录为准：
`ls C:/Users/gsjs5/.qoder-cn/plugins/cache/qoder-marketplace/diagram-design/`

```bash
PY="D:/Anaconda3/envs/classify/python.exe"
SKILL="C:/Users/gsjs5/.qoder-cn/plugins/cache/qoder-marketplace/diagram-design/2.6.131/skills/diagram-design"
CHROME="C:/Program Files/Google/Chrome/Application/chrome.exe"

# 1) 契约自检，必须全部 OK
"$PY" "$SKILL/scripts/self_check.py" diagrams/flow-tool-call.html

# 2) 导出独立 SVG（中间产物，不入库）
"$PY" "$SKILL/scripts/export_svg.py" diagrams/flow-tool-call.html "$TEMP/flow-tool-call.svg"

# 3) Chrome 无头截图，window-size 必须等于 SVG 的 viewBox，PNG 尺寸 = viewBox × 2
"$CHROME" --headless --disable-gpu --hide-scrollbars \
  --force-device-scale-factor=2 --window-size=960,400 \
  --screenshot="diagrams/flow-tool-call.png" \
  "file:///C:/Users/gsjs5/AppData/Local/Temp/flow-tool-call.svg"
```

已有图的 viewBox 与导出尺寸：

| 图 | 源文件 | viewBox | PNG |
|---|---|---|---|
| 图 1 | `llm-concept-map.html` | 960×600 | 1920×1200 |
| 图 2 | `flow-tool-call.html` | 960×400 | 1920×800 |
| 图 3 | `flow-skill-disclosure.html` | 960×480 | 1920×960 |
| 图 4 | `flow-rag-evolution.html` | 960×560 | 1920×1120 |
| 图 5 | `flow-cost-ladder.html` | 960×500 | 1920×1000 |
| 图 6 | `flow-request-lifecycle.html` | 1064×420 | 2128×840 |
| 图 7 | `flow-qoder-task.html` | 1176×420 | 2352×840 |

> 图 1 的 viewBox 以文件内实际值为准，改动后同步本表。

## 验收闸门

提交前逐条过：

1. 改动过的每个 HTML 都跑 `self_check.py`，退出码 0 且输出全为 OK。
2. 对应 PNG 已重新生成，尺寸等于 viewBox × 2，文件大小不为 0。
3. Markdown 中每个 `![...](diagrams/*.png)` 和 `[...](diagrams/*.html)` 目标都存在。
4. `diagrams/` 下没有残留 `*.svg`。
5. `git status` 只包含预期文件；必要时用 `git hash-object <file>` 与 `git rev-parse HEAD:<file>` 比对，确认落盘内容确实进了提交。
6. 视觉抽查：中文无豆腐块或乱码、accent 不超过 2 处、箭头正交且不压节点、标签遮罩生效。

## 已知坑

| 现象 | 处理 |
|---|---|
| `python3` 退出码 49、无输出 | 本机是 Windows Store 别名。一律用 `D:/Anaconda3/envs/classify/python.exe` 绝对路径，不要静默回退 |
| 想用 Playwright 导出 PNG | 本机未安装。走 `export_svg.py` + Chrome headless 两步 |
| Chrome 无头模式在 stderr 报扩展加载失败 | 与本任务无关；以 `--screenshot` 是否产出文件、尺寸是否正确为准 |
| HTML 里的 Google Fonts 拉不到 | 离线时回退系统中文字体（Microsoft YaHei / Noto Sans SC），中文仍可读；联网环境下再导出正式 PNG |
| 全局 `core.autocrlf=true` | 仓库用 `.gitattributes` 固定 Markdown/HTML 为 LF、PNG 为二进制；新增文本文件不要绕过该策略 |
| 编辑工具报成功但磁盘内容未变 | 用 Python 显式写入并复核字节数，再继续后续步骤 |
| 图里出现斜线或标签压线 | 直接重画该连接线，不要靠移动节点掩盖；规则见“改图”一节 |

## 提交约定

- 提交信息用 `docs: ` 前缀，中文说明“改了什么、为什么”，不要只写“更新文档”。
- 只 `git add` 具名文件，不要整目录无差别暂存。
- 默认不自动提交或推送；用户明确要求时，提交后推送 `main`，并核对本地与 `origin/main` 指向同一提交。
