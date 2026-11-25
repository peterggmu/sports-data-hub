# sports-data-hub

全球足球与篮球的球队、场馆、赛果结构化数据仓库。按「原始数据 → 标准化数据 → 素材 → 产物」分层管理：GitHub 为唯一数据源头，Notion 仅作为单向同步的浏览视图。

## 目录结构

```
sports-data-hub/
├── data/
│   ├── raw/              # 原始采集数据（只追加、不修改），按 来源/联赛/日期 组织
│   ├── normalized/       # 标准化数据，唯一事实来源
│   │   ├── leagues/
│   │   ├── teams/
│   │   ├── stadiums/
│   │   └── matches/
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
2. **normalized**：按 Schema 清洗后的记录，一条记录一个 JSON 文件，文件名与记录 `id` 一致。
3. **assets**：图片素材直接入库；视频等大文件走 Git LFS 或外部存储，仓库内只存路径。
4. **artifacts**：成品文件本身放 Release 或云盘，目录内只保留清单和链接。

## 数据更新流程

1. 新数据先入 `data/raw/`；
2. 按 `docs/data-dictionary.md` 清洗，并通过 `data/schemas/` 下的 Schema 校验；
3. 写入 `data/normalized/` 对应子目录，文件名 = `id.json`；
4. 提交信息写明数据范围与来源；
5. 需要时再由脚本单向同步到 Notion。

## 文档

- 数据字典与 ID 规则：[`docs/data-dictionary.md`](docs/data-dictionary.md)
- Schema 目录：[`data/schemas/`](data/schemas/)
