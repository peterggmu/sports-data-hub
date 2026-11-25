# 数据字典与 ID 规则

## 一、ID 规则

- 所有 `id` 使用小写英文 slug，单词间用连字符 `-` 连接，例如 `j1-league`、`old-trafford`。
- 联赛、球队、场馆的 `id` 一经分配不可更改；实体改名时只修改名称字段。
- 比赛 `id` 格式：`YYYY-MM-DD-主队id-客队id`，例如 `2026-11-25-team-a-team-b`。
- 引用字段（`league_id`、`stadium_id`、`home_team_id` 等）必须指向已存在的记录 `id`。
- 日期字段使用 `YYYY-MM-DD`；日期时间使用 ISO 8601 并带时区偏移，例如 `2026-11-25T19:30:00+08:00`。
- 暂缺的字段统一填 `null`，不用空字符串，也不自行编造。

## 二、通用字段

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| id | string | 全局唯一标识，slug 格式 |
| sport | string | 项目：`football` / `basketball` |
| updated_at | string | 数据最后核对日期，`YYYY-MM-DD` |

## 三、league（联赛）

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| id | string | 是 | 联赛唯一标识 |
| name_zh | string | 是 | 中文名称 |
| name_en | string | 是 | 英文名称 |
| sport | string | 是 | 所属项目 |
| confederation | string | 否 | 所属足联：AFC / UEFA / CONMEBOL / CONCACAF / CAF / OFC / FIBA |
| level | string | 否 | 联赛级别，如「顶级」「次级」 |
| founded | integer \| null | 否 | 创办年份 |
| country | string | 是 | 国家/地区 |
| updated_at | string | 是 | 最后核对日期 |

## 四、team（球队）

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| id | string | 是 | 球队唯一标识 |
| name_zh | string | 是 | 中文名称 |
| name_en | string | 是 | 英文名称 |
| sport | string | 是 | 所属项目 |
| league_id | string | 是 | 当前所属联赛 id |
| stadium_id | string \| null | 否 | 主场场馆 id |
| city | string | 否 | 所在城市 |
| country | string | 否 | 国家/地区 |
| founded | integer \| null | 否 | 成立年份 |
| crest_path | string \| null | 否 | 队徽文件相对路径，如 `assets/crests/xxx.png` |
| updated_at | string | 是 | 最后核对日期 |

## 五、stadium（场馆）

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| id | string | 是 | 场馆唯一标识 |
| name_zh | string | 是 | 中文名称 |
| name_en | string | 是 | 英文名称 |
| city | string | 是 | 所在城市 |
| country | string | 是 | 国家/地区 |
| capacity | integer | 是 | 场馆容量，单位：人 |
| opened | integer \| null | 否 | 落成年份 |
| surface | string \| null | 否 | 场地表面：grass / hybrid / artificial / hardwood |
| photo_path | string \| null | 否 | 照片相对路径，如 `assets/stadium-photos/xxx.jpg` |
| coordinates | object \| null | 否 | 经纬度，含 `lat`、`lng` |
| tenant_ids | string[] | 否 | 入驻球队 id 列表 |
| updated_at | string | 是 | 最后核对日期 |

## 六、match（比赛）

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| id | string | 是 | 比赛标识，`YYYY-MM-DD-主队-客队` |
| kickoff | string | 是 | 开球时间，ISO 8601 带时区 |
| sport | string | 是 | 所属项目 |
| league_id | string | 是 | 所属联赛 id |
| home_team_id | string | 是 | 主队 id |
| away_team_id | string | 是 | 客队 id |
| status | string | 是 | scheduled / finished / postponed / cancelled |
| home_score | integer \| null | 否 | 主队进球/得分 |
| away_score | integer \| null | 否 | 客队进球/得分 |
| stadium_id | string \| null | 否 | 比赛场馆 id |
| attendance | integer \| null | 否 | 上座人数，单位：人 |
| updated_at | string | 是 | 最后核对日期 |
