# sports-data-hub

个人通用参考数据仓库：以体育数据为主，兼及文化、社会等主题的结构化参考数据。仓库名 `sports-data-hub` 是创建时的历史名称，按既定约定保留不变；实际内容已扩展为通用参考数据库，不以仓库名限定收录范围。

## 组织方式：物理平铺 + 中央注册表

- `data/normalized/` 下所有数据集**平铺存放，不设领域子目录**，保持路径短、结构扁平。
- 每个数据集的逻辑领域（体育 / 文化 / 社会）、记录数与各类路径，统一登记在中央注册表 [`data/registry.json`](data/registry.json)，实现「物理扁平、逻辑可分类」。
- 新增或更新数据集时，必须同步更新 `registry.json`，并使其中记录数与各目录 `index.csv` 保持一致。

## 数据集清单（按领域）

### 体育 sports

| 目录 | 中文名 | 记录数 |
| --- | --- | --- |
| olympic-host-elections | 奥运会主办城市选举 | 64 |
| world-cup-bid-votes | 2026 年世界杯申办投票 | 203 |
| womens-world-cup-bid-votes | 2027 年女足世界杯申办投票 | 207 |

### 文化 culture

| 目录 | 中文名 | 记录数 |
| --- | --- | --- |
| unesco-creative-cities | 联合国教科文组织创意城市 | 406 |

### 社会 society

| 目录 | 中文名 | 记录数 |
| --- | --- | --- |
| soviet-largest-cities | 苏联最大城市 | 10 |

## 目录结构

```
sports-data-hub/
├── data/
│   ├── registry.json     # 中央数据集注册表（领域、记录数、路径、更新日期）
│   ├── raw/              # 原始采集数据（只追加、不修改），按 来源/主题/日期 组织
│   ├── normalized/       # 标准化数据，唯一事实来源（平铺，不设领域子目录）
│   │   ├── olympic-host-elections/
│   │   ├── world-cup-bid-votes/
│   │   ├── womens-world-cup-bid-votes/
│   │   ├── unesco-creative-cities/
│   │   └── soviet-largest-cities/
│   └── schemas/          # JSON Schema 字段规范
├── assets/               # 队徽、球场照片等素材
├── artifacts/            # PPT、海报、视频等产物的清单与外链
├── scripts/              # 采集、清洗、校验、Notion 同步脚本
└── docs/                 # 数据字典与规则说明
```

## 分层规则

1. **raw**：原样保存采集结果，永不覆盖，便于回溯重跑。
2. **normalized**：按 Schema 清洗后的记录，是唯一事实来源。每个数据集目录遵循统一的「实体目录标准构成」（必备 `index.csv` 总表、一个或多个 JSON 数据文件，并在 `data/schemas/` 有对应 Schema），详见 [`docs/data-dictionary.md`](docs/data-dictionary.md)。
3. **registry**：不存放数据本身，只登记数据集元信息与路径，是浏览和检索全库的入口。
4. **assets**：图片素材直接入库；视频等大文件走 Git LFS 或外部存储，仓库内只存路径。
5. **artifacts**：成品文件本身放 Release 或云盘，目录内只保留清单和链接。

## 数据更新流程

1. 新数据先入 `data/raw/`；
2. 按 `docs/data-dictionary.md` 清洗，并通过 `data/schemas/` 下对应 Schema 校验；
3. 写入 `data/normalized/` 对应目录，同时更新该目录的 `index.csv`；
4. 在 `data/registry.json` 同步登记或更新该数据集的领域、记录数、路径与更新日期；
5. 提交信息写明数据范围与来源；
6. 需要时再由脚本单向同步到 Notion。

## 文档

- 数据字典与 ID 规则：[`docs/data-dictionary.md`](docs/data-dictionary.md)
- 中央数据集注册表：[`data/registry.json`](data/registry.json)
- 奥运主办城市选举总表：[`data/normalized/olympic-host-elections/index.csv`](data/normalized/olympic-host-elections/index.csv)
- 2026 世界杯申办投票明细：[`data/normalized/world-cup-bid-votes/index.csv`](data/normalized/world-cup-bid-votes/index.csv)
- 2027 女足世界杯申办投票明细：[`data/normalized/womens-world-cup-bid-votes/index.csv`](data/normalized/womens-world-cup-bid-votes/index.csv)
- UNESCO 创意城市总表：[`data/normalized/unesco-creative-cities/index.csv`](data/normalized/unesco-creative-cities/index.csv)
- 苏联十大城市总表：[`data/normalized/soviet-largest-cities/index.csv`](data/normalized/soviet-largest-cities/index.csv)
- Schema 目录：[`data/schemas/`](data/schemas/)
