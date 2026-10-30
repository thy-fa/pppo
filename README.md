# 大数据招聘岗位数据分析

> 基于 **3357 条真实招聘数据**的全链路数据分析项目：采集 → 清洗 → 分析 → 可视化 → 决策建议。
> 用数据回答应届生最关心的三个问题：**去哪个城市、学什么技能、期望薪资多少**。

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue)](https://www.python.org/)
[![pandas](https://img.shields.io/badge/pandas-清洗%2F分析-150458)](https://pandas.pydata.org/)
[![matplotlib](https://img.shields.io/badge/matplotlib-可视化-orange)](https://matplotlib.org/)
[![样本量](https://img.shields.io/badge/样本-3357%20条-green)](https://www.heywhale.com/)
[![License](https://img.shields.io/badge/License-MIT-lightgrey)](https://opensource.org/licenses/MIT)

**作者**：专科 · 大数据技术专业 · 大三　**用途**：求职作品集项目

> ### 在线浏览
>
> 本项目已配置 GitHub Pages，**站点根目录为 `docs/`**：
>
> - 站点首页：`https://<用户名>.github.io/<仓库名>/` → 由 `docs/index.md` 渲染
> - 完整报告（推荐）：`https://<用户名>.github.io/<仓库名>/project_report.html`
> - 报告 Markdown 版：`https://<用户名>.github.io/<仓库名>/report.html`
>
> ⚠️ 注意：Markdown 在 Pages 上渲染后的地址是 **`.html`**，不是 `.md`。

---

## 一图看懂

| 图1 城市机会与薪资 | 图2 学历 × 经验薪资矩阵 |
| :---: | :---: |
| ![城市](charts/city_salary.svg) | ![学历经验](charts/edu_exp_salary.svg) |
| 岗位最多的城市 ≠ 薪资最高的城市 | 薪资随经验呈台阶式跃迁 |

| 图3 高频技能 Top15 | 图4 薪资分布与定位 |
| :---: | :---: |
| ![技能](charts/skill_freq.svg) | ![分布](charts/salary_dist.svg) |
| 技能需求差异大，但薪资极差仅 1.25K | 期望薪资该锚定哪个分位 |

---

## 四条核心结论

| # | 结论 | 关键数字 |
| ---: | :--- | :--- |
| 1 | **经验溢价远大于学历溢价** | 无需经验→1年经验 **+133%**；大专→本科 仅 +29% |
| 2 | **岗位数量 ≠ 薪资水平** | 南京 348 条 / 14.0K vs 深圳 257 条 / 20.0K |
| 3 | **技能决定能否进门，不决定薪资高低** | 各技能薪资中位数集中在 17.5K–18.8K，极差仅 1.25K |
| 4 | **期望薪资锚定 P25 而非中位数** | P25 13.3K ｜ 中位 18.0K（由 3-4 年经验撑起） |

> 结论 3 是本项目最有价值的发现：需求侧 SQL 40.5%、Java 40.0% 与 Flink 8.3% 差距巨大，
> 但薪资侧几乎无差异——说明**技能是准入门槛，不是溢价来源**。

---

## 项目亮点

- **全链路闭环**：独立打通"采集—清洗—分析—可视化"，而非只做单点脚本
- **真实脏数据处理**：31% 重复记录、475 条坏行、1500+ 处转义符污染，清洗规则均基于实际统计而非凭经验设定
- **口径意识**：识别并统一"月薪 / 年薪"双口径（2860 + 497 = 3357 互补），薪资可用率提升至 100%
- **工程化细节**：双引擎读取（pandas / 标准库降级）、依赖缺失时的文本模式兜底、断点续采、频率控制
- **合规判断**：主动识别 robots 对 `/api/` 的禁止约定，放弃自动下载方案，改走手动下载 + 脚本标准化

---

## 快速开始

```bash
# 1. 获取数据集（需注册登录，详见 data/README.md）
#    下载 清洗后.csv / job_all.csv 后放到项目根目录或 data/ 下

# 2. 安装依赖
pip install pandas matplotlib openpyxl

# 3. 生成 4 张图表到 charts/
python src/job_visualization.py          # 需要 matplotlib，输出 PNG

#    无 matplotlib 也能出图（纯标准库，输出 SVG）：
python src/export_charts_svg.py

#    若数据文件不在默认位置，显式指定：
python src/job_visualization.py --input "你的路径/清洗后job.xlsx"

#    小样本先跑通：
python src/job_visualization.py --limit 500
```

> 脚本会自动在 `./清洗后job.xlsx`、`data/清洗后job.xlsx` 等位置查找输入文件；
> 未安装 pandas / matplotlib 时会自动降级（标准库解析 + 文本模式输出），不会直接报错退出。

产出：

| 文件 | 内容 |
| :--- | :--- |
| `charts/city_salary.svg` | 图1 城市机会与薪资（双轴） |
| `charts/edu_exp_salary.svg` | 图2 学历 × 经验薪资矩阵 |
| `charts/skill_freq.svg` | 图3 高频技能 Top15 |
| `charts/salary_dist.svg` | 图4 薪资分布与定位 |
| `charts/viz_summary.csv` | 各维度汇总表 |

---

## 目录结构

```
.
├── README.md                  # 本文件（仓库首页）
├── .gitignore                 # 排除原始数据（*.xlsx / *.csv）与缓存
├── docs/                      # ← GitHub Pages 站点根目录
│   ├── index.md               # 站点首页（访问根 URL 时渲染）
│   ├── project_report.html    # 完整报告 HTML 版（自包含，推荐入口）
│   ├── report.md              # 完整报告 Markdown 版（六章 + 简历附录）
│   ├── visualization.html     # 可视化方案专页
│   ├── pitch_script.md        # 2 分钟面试口述稿
│   └── interview_qa.md        # 33 道面试问答
├── charts/                    # 4 张图表（SVG，共约 30KB）+ 汇总 CSV
├── src/
│   ├── job_crawler.py         # 采集脚本（requests + bs4，含 robots 校验）
│   ├── clean_eda_pipeline.py  # 清洗 + EDA（pandas）
│   ├── job_visualization.py   # 可视化流水线（matplotlib）
│   ├── heywhale_dataset_loader.py  # 数据集合规获取与标准化
│   └── export_charts_svg.py   # 图表导出为 SVG（纯标准库，无依赖）
└── data/
    └── README.md              # 数据源说明与获取方式
```

---

## 技术栈

| 环节 | 工具 | 说明 |
| :--- | :--- | :--- |
| 数据采集 | `requests` + `BeautifulSoup` | 含 robots 校验、频率控制、断点续采 |
| 数据存储 | CSV（原始层）/ SQLite（标准层） | 原始层与清洗层分离，可回溯 |
| 数据清洗 | `pandas` | 去重、缺失分策、类型解析、异常值截尾 |
| 数据分析 | `pandas` | 描述统计、分组统计、关键词匹配 |
| 数据可视化 | `matplotlib` | 4 张图表，统一中文字体与配色 |

---

## 数据源与合规声明

- **数据来源**：和鲸社区公开数据集《前程无忧 - 大数据岗位数据集》（3357 条 × 21 字段，2022 年 11 月）
- **获取方式**：手动下载后由脚本标准化处理
- **合规说明**：
  - 目标站点 `robots.txt` 明确 `Disallow: /api/`，文件下载接口位于该路径下，
    因此**未使用任何自动化下载手段**
  - 数据仅用于个人学习研究，不作商业用途
  - 对外仅展示聚合统计结果，不分发原始数据集

---

## 文档导航

| 文档 | 说明 |
| :--- | :--- |
| [docs/index.md](docs/index.md) | GitHub Pages 站点首页 |
| [docs/project_report.html](docs/project_report.html) | 完整报告（六章，含内嵌图表，可打印 PDF） |
| [docs/report.md](docs/report.md) | 完整报告 Markdown 版 |
| [docs/visualization.html](docs/visualization.html) | 可视化方案专页 |
| [docs/pitch_script.md](docs/pitch_script.md) | 2 分钟面试口述稿 |
| [docs/interview_qa.md](docs/interview_qa.md) | 33 道面试问答清单 |

简历项目经历描述见报告附录，可直接复制粘贴。

---

## GitHub Pages 部署（避免 404 的关键）

**Settings → Pages → Build and deployment → Source**，两项必须与本仓库结构匹配：

| 设置项 | 应选值 | 选错的后果 |
| :--- | :--- | :--- |
| Source | **Deploy from a branch** | 若选 GitHub Actions 且无 workflow，站点永不构建 |
| Branch | **`main`（你 push 的分支）** + **`/docs`** | 选 `/(root)` 会因根目录无 index 文件而 404 |

其他必要条件：

- 仓库必须为 **Public**（免费账号的私有仓库无法启用 Pages）
- `docs/` 下必须有 `index.md` 或 `index.html`，否则根 URL 无法渲染
- 首次部署需等待 1–10 分钟；修改配置后同样需要等待
- Markdown 渲染后的访问地址是 `report.html`，而非 `report.md`

---

## License

MIT License. 数据版权归原数据集发布方所有，本项目仅用于学习研究。
