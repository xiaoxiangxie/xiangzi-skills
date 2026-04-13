# CookLikeHOC Skill

本 Skill 基于开源项目 [Gar-b-age/CookLikeHOC](https://github.com/Gar-b-age/CookLikeHOC) 构建，将其核心的菜谱速查能力无缝集成到大模型 Agent 平台中。

## 调用说明与引用内容

- **调用的源仓库**：[Gar-b-age/CookLikeHOC](https://github.com/Gar-b-age/CookLikeHOC) (主要收录《老乡鸡菜品溯源报告》中的菜谱 Markdown)。
- **引用的内容**：动态拉取该仓库中存放在 `主食`、`炒菜`、`汤`、`炸品` 等十余个目录下的 `.md` 菜谱文件（涵盖配料和步骤说明）。
- **完全解耦**：本 Skill **并未直接打包任何源文件的菜谱到包内**，因此总大小极小（仅不到 5KB），能够时刻随上游项目同步最新菜品变化。

## 具体实现方式

本技能由两部分核心文件组成：
1. `SKILL.md`: 提供给 AI 阅读的触发器说明，指导大语言模型如何在自然语言对话（如“我要像老乡鸡那样做饭”）时正确执行下层脚本。
2. `scripts/search_recipe.py`: 核心查询执行脚本。

### `search_recipe.py` 的架构与特点：
- **无外部依赖**：强制只使用 Python 自带的 `urllib`、`json`、`difflib` 标准库，保证 Skill 在任何环境下即拉即用。
- **中国网络优化**：弃用了国内难以访问的原版 `raw.githubusercontent.com`，采用针对中国大陆访问极速稳定的 `fastly.jsdelivr.net` 作为官方内容分发 CDN。
- **双重缓存保护层**：为了绕过 GitHub 给匿名 IP 设定的每小时 60 次 API (api.github.com) 访问限流，我们在脚本初始化时抓取一次完整目录，并将其缓存为本地 `recipe_tree_cache.json`（有效期 24 小时）。即使在完全断网或被 GitHub 暂时 Block 的情况下也保证查询不断供。
- **智能模糊匹配**：如果用户搜索的“牛肉面”没有完全重合的名字，但库内有“香辣牛肉面”，其内置的 `get_close_matches` 会给出最匹配的可用选项，防呆容错拉满。
