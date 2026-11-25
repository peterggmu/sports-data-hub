# sports-data-hub

全球体育数据仓库

## 目录结构

```
sports-data-hub/
├── data/
│   ├── raw/              # 原始采集数据（只追加、不修改），按 来源/主题/日期 组织
│   ├── normalized/       # 标准化数据，唯一事实来源
│   │   ├── leagues/
│   │   ├── teams/
│   │   ├── stadiums/
│   │   ├── matches/
│   │   ├── olympic-host-elections/
│   │   ├── world-cup-bid-votes/
│   │   └── womens-world-cup-bid-votes/
│   └── schemas/          # JSON Schema 字段规范
├── assets/               # 队徽、球场照片等素材
│   ├── crests/
│   └── stadium-photos/
├── artifacts/            # PPT、海报、视频等产物的清单与外链
├── scripts/              # 采集、清洗、校验、Notion 同步脚本
└── docs/                 # 数据字典与规则说明
```

## 分层规则

1. **raw**：原样保存采集结果，永不覆盖，便于回溯重跑。
2. **normalized**：按 Schema 清洗后的记录。独立实体一条一个 JSON，文件名与记录 `id` 一致；投票名册类数据按事件存为一个 JSON 并配明细 CSV；每个实体目录同时维护 `index.csv` 总表。
3. **assets**：图片素材直接入库；视频等大文件走 Git LFS 或外部存储，仓库内只存路径。
4. **artifacts**：成品文件本身放 Release 或云盘，目录内只保留清单和链接。

## 数据更新流程

1. 新数据先入 `data/raw/`；
2. 按 `docs/data-dictionary.md` 清洗，并通过 `data/schemas/` 下的 Schema 校验；
3. 写入 `data/normalized/` 对应目录，同时更新该目录的 `index.csv`；
4. 提交信息写明数据范围与来源；
5. 需要时再由脚本单向同步到 Notion。

## 文档

- 数据字典与 ID 规则：[`docs/data-dictionary.md`](docs/data-dictionary.md)
- 奥运主办城市选举总表：[`data/normalized/olympic-host-elections/index.csv`](data/normalized/olympic-host-elections/index.csv)
- 2026 世界杯申办投票明细：[`data/normalized/world-cup-bid-votes/index.csv`](data/normalized/world-cup-bid-votes/index.csv)
- 2027 女足世界杯申办投票明细：[`data/normalized/womens-world-cup-bid-votes/index.csv`](data/normalized/womens-world-cup-bid-votes/index.csv)
- Schema 目录：[`data/schemas/`](data/schemas/)
