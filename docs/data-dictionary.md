# 数据字典与 ID 规则

## 一、ID 规则

- 所有 `id` 使用小写英文 slug，单词间用连字符 `-` 连接，例如 `j1-league`、`old-trafford`。
- 联赛、球队、场馆的 `id` 一经分配不可更改；实体改名时只修改名称字段。
- 比赛 `id` 格式：`YYYY-MM-DD-主队id-客队id`，例如 `2026-11-25-team-a-team-b`。
- 奥运主办城市选举 `id` 格式：`届次年份-summer/winter`，例如 `1936-summer`、`2022-winter`。
- 世界杯申办投票文件 `id` 格式：`YYYY-world-cup-bid`。
- 女足世界杯申办投票文件 `id` 格式：`YYYY-womens-world-cup-bid`。
- UNESCO 创意城市 `id` 格式：`类别slug-城市英文slug`；城市 slug 去除重音符、非字母数字统一转连字符，例如 `literature-edinburgh`、`music-ghent`、`crafts-folk-art-santa-fe`。
- 苏联最大城市 `id` 格式：`soviet-largest-cities-当时城市英文slug`，例如 `soviet-largest-cities-moscow`、`soviet-largest-cities-leningrad`。
- 引用字段（`league_id`、`stadium_id`、`home_team_id` 等）必须指向已存在的记录 `id`。
- 日期字段使用 `YYYY-MM-DD`；日期时间使用 ISO 8601 并带时区偏移，例如 `2026-11-25T19:30:00+08:00`。
- 暂缺的字段统一填 `null`，不要留空字符串或自行编造。

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
| country | string | 是 | 国家/地区 |
| confederation | string | 否 | 所属足联：AFC / UEFA / CONMEBOL / CONCACAF / CAF / OFC / FIBA |
| level | string | 否 | 联赛级别，如「顶级」「次级」 |
| founded | integer \| null | 否 | 创办年份 |
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

## 七、olympic-host-election（奥运主办城市选举）

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| id | string | 是 | `届次年份-summer/winter` |
| game_year | integer | 是 | 奥运届次年份 |
| game_type | string | 是 | `summer`（夏季）/ `winter`（冬季） |
| host_city_zh | string | 是 | 最终获选城市/地区，联合主办保留连接号 |
| original_host_zh | string \| null | 否 | 最初获选、后来退出或更换的城市 |
| host_changed | boolean | 是 | 主办地是否发生变更 |
| vote_type | string | 是 | 选举类型，见下方枚举说明 |
| vote_summary_zh | string | 是 | 投票结果原文，逐字保留 |
| confirmation_votes | object \| null | 否 | 确认/提案表决票数：`yes` 赞成、`no` 反对、`abstain` 弃权 |
| candidates | object[] | 是 | 候选城市列表，无票数记录时为空数组 |
| candidates[].city_zh | string | 是 | 候选城市/地区名称 |
| candidates[].round_votes | integer[] | 是 | 各轮得票，按原文顺序；城市被淘汰后不再出现后续轮次，故不强行对齐轮次 |
| candidates[].runoff_votes | integer \| null | 否 | 加赛得票 |
| games_cancelled | boolean | 是 | 该届赛事是否取消 |
| notes_zh | string \| null | 否 | 补充说明 |
| updated_at | string | 是 | 最后核对日期 |

`vote_type` 枚举含义：

- `acclamation`：鼓掌通过 / 一致通过
- `competitive`：有候选城市之间的竞争性投票
- `confirmation`：确认性表决或整体提案（含赞成/反对/弃权）
- `direct`：直接确定，无竞争性投票
- `mail`：邮寄表决
- `no_election`：未举行主办地选举

## 八、world-cup-bid-vote（世界杯申办投票）

整个文件描述一次世界杯主办权投票事件，含元信息、结果统计与全部选票记录。

顶层字段：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| id | string | 是 | 格式 `YYYY-world-cup-bid` |
| tournament | string | 是 | 对应世界杯名称 |
| vote_date | string | 是 | 投票日期 |
| congress | string | 否 | FIFA 大会届次 |
| location | string | 是 | 投票地点 |
| winning_bid | string | 否 | 胜出申办方 |
| vote_legend | object | 是 | 投票代码含义，含 U/M/N/A 四项 |
| result | object | 是 | 结果统计 |
| records | object[] | 是 | 各足协选票 |
| updated_at | string | 是 | 最后核对日期 |

`result` 字段：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| eligible_associations | integer | 有投票资格的协会数 |
| valid_votes | integer | 有效票数 |
| united_bid | integer | 联合申办得票 |
| morocco | integer | 摩洛哥得票 |
| none_of_the_bids | integer | 「两个申办都不支持」得票 |
| abstentions | integer | 弃权数（不计入有效票） |
| majority_required | integer | 当选所需票数 |

`records` 字段：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| confederation | string | 是 | 所属足联：AFC / CAF / CONCACAF / CONMEBOL / OFC / UEFA |
| association_en | string | 是 | 足协英文名称 |
| vote | string | 是 | 选票代码，见下 |

`vote` 代码含义：

- `U`：投联合申办（加拿大 / 墨西哥 / 美国）
- `M`：投摩洛哥
- `N`：选择「两个申办都不支持」（None of the bids），计入有效票
- `A`：弃权（Abstained），不计入有效票

## 九、womens-world-cup-bid-vote（女足世界杯申办投票）

整个文件描述一次女足世界杯主办权投票事件，含元信息、申办方、结果统计、全部选票记录与数据质量说明。

顶层字段：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| id | string | 是 | 格式 `YYYY-womens-world-cup-bid` |
| tournament | string | 是 | 对应女足世界杯名称 |
| edition | string | 否 | 赛事届次，如 `10th` |
| vote_date | string | 是 | 投票日期 |
| congress | string | 否 | FIFA 大会届次 |
| location | string | 是 | 投票地点 |
| ballot_type | string | 否 | 投票方式，如 `open ballot`（公开投票） |
| winning_bid | string | 否 | 胜出申办方 |
| vote_legend | object | 是 | 投票代码含义，含 BRA/BNG/NAN/ABS 四项 |
| bids | object[] | 否 | 申办方列表，含 `code`、`name_en` |
| result | object | 是 | 结果统计 |
| records | object[] | 是 | 各足协选票 |
| data_quality_notes | string[] | 否 | 数据修正与口径说明，透明记录 |
| updated_at | string | 是 | 最后核对日期 |

`result` 字段：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| eligible_associations | integer | 有投票资格的协会数 |
| votes_for_bids | integer | 投向申办方的票数（不含 NAN 与弃权） |
| brazil | integer | 巴西得票 |
| bng | integer | 比荷德联合申办得票 |
| none_of_the_bids | integer | 「两个申办都不支持」数 |
| abstentions | integer | 弃权数 |
| majority_required | integer | 当选所需票数 |

`records` 字段：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| confederation | string | 是 | 所属足联：AFC / CAF / CONCACAF / CONMEBOL / OFC / UEFA |
| association_en | string | 是 | 足协英文名称 |
| vote | string | 是 | 选票代码，见下 |

`vote` 代码含义：

- `BRA`：投巴西（Brazil）
- `BNG`：投比利时 / 荷兰 / 德国联合申办（Belgium-Netherlands-Germany）
- `NAN`：选择「两个申办都不支持」（None of the bids）
- `ABS`：弃权（Abstained）

数据质量约定：当原始数据与 FIFA 官方逐协会记录不一致时，`normalized` 层按官方口径修正，并在 `data_quality_notes` 中逐条说明；`raw` 层始终原样保留，不做改动。

## 十、unesco-creative-city（UNESCO 创意城市）

记录 UNESCO 创意城市网络（Creative Cities Network，2004 年启动）成员的创意领域、城市、国家与命名年份。称号为永久称号。

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| id | string | 是 | `类别slug-城市英文slug`，全局唯一 |
| category_en | string | 是 | 创意领域英文名，见下方枚举 |
| category_zh | string | 是 | 创意领域中文名 |
| city_en | string | 是 | 城市英文名 |
| city_zh | string | 是 | 城市中文名 |
| country_en | string | 是 | 国家/地区英文名 |
| country_zh | string | 是 | 国家/地区中文名 |
| year | integer | 是 | 加入/命名年份（按官方批次） |
| notes_zh | string \| null | 否 | 补充说明，如转换领域、年份修正 |
| updated_at | string | 是 | 最后核对日期 |

`category` 枚举（8 个创意领域）：

- `Architecture`：建筑之都（2025 年新设，首批 5 城）
- `Crafts and Folk Art`：手工艺与民间艺术之都
- `Design`：设计之都
- `Film`：电影之都
- `Gastronomy`：美食之都
- `Literature`：文学之都
- `Media Arts`：媒体艺术之都
- `Music`：音乐之都

存储与口径约定：

- `normalized/unesco-creative-cities/` 按类别拆分为 8 个 JSON 文件（`architecture.json`、`crafts-folk-art.json`、`design.json`、`film.json`、`gastronomy.json`、`literature.json`、`media-arts.json`、`music.json`），每个文件为该类别记录数组；`index.csv` 为全部记录总表。
- 共 406 条类别记录、405 个唯一城市；格拉纳达（Granada）是唯一拥有两个领域的城市（文学之都 2014、设计之都 2023），故保留两条记录。
- 里昂（Lyon）2008 年以媒体艺术之都入会，2023 年应其申请转为文学之都，`year` 保留入会年 2008，并在 `notes_zh` 说明。
- 概念区分：UNESCO「世界图书之都」（World Book Capital）为每年一个、为期一年的独立项目，不属于本表，不并入。
- 官方不同来源对总记录数存在 407/408 的口径差异（含历史领域记录与更新时间差）；本表按「当前领域」合并为 405 个唯一城市，不补造未公布城市。

## 十一、soviet-largest-city（1989 年苏联十大城市）

记录 1989 年苏联人口普查中人口最多的十个城市，含当时名称、现用名、当时加盟共和国、现属国家与 1979/1989 两次普查人口。

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| id | string | 是 | `soviet-largest-cities-` + 当时城市英文 slug |
| rank | integer | 是 | 按 1989 年普查人口排名，1–10 |
| city_zh | string | 是 | 1989 年时城市中文名 |
| city_ru | string | 是 | 1989 年时城市俄文名（西里尔字母） |
| city_en | string | 是 | 1989 年时城市英文名（苏联时期罗马化拼写） |
| current_city_zh | string | 是 | 现用中文名；未改名者与 city_zh 相同 |
| current_city_en | string | 是 | 现用英文名；未改名者与 city_en 相同 |
| republic_zh | string | 是 | 当时所属加盟共和国中文名 |
| country_zh | string | 是 | 现属主权国家中文名 |
| country_en | string | 是 | 现属主权国家英文名 |
| population_1989 | integer | 是 | 1989 年普查人口 |
| population_1979 | integer | 是 | 1979 年普查人口 |
| change_rate | number | 是 | 变化率小数，=(1989-1979)/1979，保留三位小数 |
| change_pct | string | 是 | 变化率百分比字符串，如 12.0% |
| updated_at | string | 是 | 最后核对日期 |

名称与口径约定：

- 解体后三处城市改名：列宁格勒 → 圣彼得堡（1991）、高尔基 → 下诺夫哥罗德（1990）、斯维尔德洛夫斯克 → 叶卡捷琳堡（1991）。
- 基辅、哈尔科夫的英文罗马化由苏联时期的 Kiev、Kharkov 改为乌克兰官方拼写 Kyiv、Kharkiv，中文不变。
- 1989 年为苏联最后一次全联盟普查；十城分属 5 个加盟共和国：俄罗斯 5、乌克兰 2，乌兹别克、白俄罗斯、格鲁吉亚各 1。
- 人口与变化率均按原表口径，未做外部调整；变化率经重算核对无误。
