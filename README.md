# Bilibili API（B站 API）

[English](#english) · [中文](#中文)

## English

This repo reads public Bilibili data with Python or curl. You can search videos by keyword, read video details and comments, and look up an uploader's profile and videos. You need one EveryInfra API key. You do not need a Bilibili account or cookie. The same API also works as an MCP tool in Claude and Cursor. Billing is per call: these 5 capabilities cost $0.56 per 1,000 calls (about $0.00056 per call). Failed calls and empty results are not charged. The EveryInfra Bilibili API has 8 capabilities, all listed on the platform page: <https://everyinfra.com/en/bilibili-api?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

### Quick start

```bash
git clone https://github.com/everyinfra/bilibili-api.git
cd bilibili-api
pip install -r requirements.txt
export EVERYINFRA_API_KEY=<your API key>
python main.py search 露营装备 --out videos.csv
python main.py video "<video url>" --out video.json
python main.py comments "<video url>" --out comments.csv
python main.py user <user_id>
python main.py posts <user_id> --out posts.csv
```

- Take the video link from the `url` field of a `search` result, for example https://www.bilibili.com/video/<BV id>.
- For `user` and `posts`, use the `author_id` from a `search` result as `user_id`. It is the number in the uploader's page link space.bilibili.com/<mid>.
- Without `--out`, the command prints the first 3 rows as JSON. With `--out`, it saves a .csv or .json file. The CSV has a BOM, so Excel opens it without garbled text.
- Register at everyinfra.com, then create the API key in the console. Steps: <https://everyinfra.com/en/docs?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api#quickstart>

### curl

```bash
curl -X POST https://api.everyinfra.com/api/v1/social \
  -H "Authorization: Bearer $EVERYINFRA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"platform":"bilibili","action":"search","params":{"keyword":"露营装备"}}'
```

All capabilities use the same URL. Change `action` and `params` to call another one. Examples for `video`, `comments`, `profile`, and `user_posts` are in [examples/curl.sh](examples/curl.sh).

### Claude and Cursor (MCP)

The remote MCP URL is <https://api.everyinfra.com/mcp>. Add the `Authorization: Bearer` header with your API key. EveryInfra runs the server, so you install no local program.

Claude Code:

```bash
claude mcp add --transport http everyinfra https://api.everyinfra.com/mcp --header "Authorization: Bearer <API Key>"
```

Cursor and other clients that read `mcp.json` ([examples/mcp.json](examples/mcp.json)):

```json
{
  "mcpServers": {
    "everyinfra": {
      "url": "https://api.everyinfra.com/mcp",
      "headers": {
        "Authorization": "Bearer <API Key>"
      }
    }
  }
}
```

Custom connectors in Claude on the web and desktop, and ChatGPT, accept only a URL. Enter <https://api.everyinfra.com/mcp/all>, sign in to EveryInfra when asked, and allow access. You do not need to enter a key. Full steps: <https://everyinfra.com/en/mcp-server?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

### Price

| `action` | Per 1,000 calls |
|---|---|
| `search` | $0.56 |
| `video` | $0.56 |
| `comments` | $0.56 |
| `profile` | $0.56 |
| `user_posts` | $0.56 |

- Billing is per call, about $0.00056 each. Failed calls, empty results, parameter errors, and authentication failures are not charged.
- A new account gets a $1 trial allowance, valid for 24 hours. No credit card is needed. How to claim it: <https://everyinfra.com/en/pricing?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>
- Prices for the other 3 Bilibili capabilities are on the platform page: <https://everyinfra.com/en/bilibili-api?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

### Fields

The fields of each capability are documented on the English pages linked below.

- `search`: <https://everyinfra.com/en/bilibili-api/search?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>
- `video`: <https://everyinfra.com/en/bilibili-api/video?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>
- `comments`: <https://everyinfra.com/en/bilibili-api/comments?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>
- `profile`: <https://everyinfra.com/en/bilibili-api/profile?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>
- `user_posts`: <https://everyinfra.com/en/bilibili-api/user_posts?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

### FAQ

**Do I need a Bilibili account or cookie to call the API?**

No. Send the `Authorization: Bearer` header with your API key. The request body holds `platform`, `action`, and the capability parameters. You do not scan a QR code, keep a login session, or open a browser.

**How much does it cost? Are failed calls charged? Is there a free allowance?**

Billing is per call, from $0.56 per 1,000 calls. Failed calls, empty results, parameter errors, and authentication failures are not charged. A new account gets a $1 trial allowance, valid for 24 hours. No credit card is needed. How to claim it: <https://everyinfra.com/en/pricing?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

**Can I connect the Bilibili API to Claude or Cursor?**

Yes. The remote MCP URL is <https://api.everyinfra.com/mcp>. Add the `Authorization: Bearer` header with your API key. You install no local program. Setup for Claude, Cursor, ChatGPT, and Claude Code: <https://everyinfra.com/en/mcp-server?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>. An agent can then search Bilibili content, read comments, and look up accounts by itself.

**What legal points apply when I use Bilibili data?**

The EveryInfra Terms of Service require you to make sure your use of the data follows applicable law and the terms of the relevant platform. Do not use the data for anything illegal or anything that violates privacy or intellectual property rights. Do not use bulk scraping for harassment, fraud, or other abuse. When you use personal information such as nicknames and avatars, keep only the fields your business needs.

**Which Bilibili data can I get?**

The EveryInfra Bilibili API has 8 capabilities:

- keyword search
- profile or object details
- user content
- video details
- comments
- on-screen video comments
- subtitles
- popular

Parameters and return fields for each capability are on the platform page: <https://everyinfra.com/en/bilibili-api?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

### Tested

Before release, we ran the 5 commands in Quick start against the live API with this repo's code.

| Command | Time (Singapore, UTC+8) | Rows | Charged |
|---|---|---|---|
| `python main.py search 露营装备 --out videos.csv` | 2026-11-21 09:24 | 20 | $0.000556 |
| `python main.py video "<video url>" --out video.json` | 2026-11-21 09:24 | 1 | $0.000556 |
| `python main.py comments "<video url>" --out comments.csv` | 2026-11-21 09:25 | 10 | $0.000556 |
| `python main.py user <user_id>` | 2026-11-21 09:25 | 1 | $0.000556 |
| `python main.py posts <user_id> --out posts.csv` | 2026-11-21 09:26 | 20 | $0.000556 |

## 中文

本仓库用 Python 或 curl 读 B站公开数据：按关键词搜视频，读视频详情和评论，查 UP 主主页和投稿。只需一把 EveryInfra API Key，不用 B站账号和 Cookie，也能作为 MCP 工具接进 Claude、Cursor。按调用次数计费，这 5 个能力每千次 $0.56（每次约 $0.00056），失败和空结果不收费。B站共 8 个能力：<https://everyinfra.com/bilibili-api?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

### 快速开始

```bash
git clone https://github.com/everyinfra/bilibili-api.git
cd bilibili-api
pip install -r requirements.txt
export EVERYINFRA_API_KEY=<你的 API Key>
python main.py search 露营装备 --out videos.csv
python main.py video "<视频链接>" --out video.json
python main.py comments "<视频链接>" --out comments.csv
python main.py user <user_id>
python main.py posts <user_id> --out posts.csv
```

- 视频链接用 `search` 结果里的 `url`，形如 https://www.bilibili.com/video/<BV号>。
- `user` 和 `posts` 的 `user_id` 用 `search` 结果里的 `author_id`，就是 UP 主主页 space.bilibili.com/<mid> 里的数字。
- 不加 `--out` 时打印前 3 条 JSON；加 `--out` 存成 .csv 或 .json。CSV 带 BOM，Excel 直接打开不乱码。
- API Key 在 everyinfra.com 注册后到控制台创建，步骤见 <https://everyinfra.com/docs?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api#quickstart>

### 用 curl 调

```bash
curl -X POST https://api.everyinfra.com/api/v1/social \
  -H "Authorization: Bearer $EVERYINFRA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"platform":"bilibili","action":"search","params":{"keyword":"露营装备"}}'
```

所有能力都发到同一个地址，换 `action` 和 `params` 就行；`video`、`comments`、`profile`、`user_posts` 的写法见 [examples/curl.sh](examples/curl.sh)。

### 接进 Claude、Cursor（MCP）

远程 MCP 地址是 <https://api.everyinfra.com/mcp>，请求头加 `Authorization: Bearer` 和 API Key；服务器由 EveryInfra 运行，不用装本地程序。

Claude Code：

```bash
claude mcp add --transport http everyinfra https://api.everyinfra.com/mcp --header "Authorization: Bearer <API Key>"
```

Cursor 等读 `mcp.json` 的客户端（[examples/mcp.json](examples/mcp.json)）：

```json
{
  "mcpServers": {
    "everyinfra": {
      "url": "https://api.everyinfra.com/mcp",
      "headers": {
        "Authorization": "Bearer <API Key>"
      }
    }
  }
}
```

Claude 网页版和桌面版的自定义连接器、ChatGPT 只能填网址：填 <https://api.everyinfra.com/mcp/all>，连接时登录 EveryInfra 并允许，不用填 Key。完整步骤见 <https://everyinfra.com/mcp-server?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

### 价格

| 能力 | `action` | 每千次 |
|---|---|---|
| 关键词搜索 | `search` | $0.56 |
| 视频详情 | `video` | $0.56 |
| 评论 | `comments` | $0.56 |
| 主页或对象详情 | `profile` | $0.56 |
| 用户内容 | `user_posts` | $0.56 |

- 按调用次数计费，每次约 $0.00056；失败、空结果、参数错误和鉴权失败都不收费。
- 新账号有 $1 试用额度，24 小时内有效，不用信用卡，领取方式见 <https://everyinfra.com/pricing?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>
- B站其他 3 个能力的单价见平台页 <https://everyinfra.com/bilibili-api?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

### 返回字段

下面是这 5 个能力实测返回的字段，含义取自接口文档每条说明的第一句，完整说明在每张表下面的链接里。

#### `search`（关键词搜索）

| 字段 | 含义 |
|---|---|
| `id` | 该记录在原平台上的唯一标识。 |
| `url` | 该记录在原平台上的可访问链接。 |
| `title` | 标题。 |
| `duration_seconds` | 时长，单位秒。 |
| `duration_text` | 时长的展示文本（如 12:34）。 |
| `view_count` | 浏览/播放数。 |
| `like_count` | 点赞数。 |
| `comment_count` | 评论数。 |
| `danmaku_count` | 弹幕数。 |
| `favorite_count` | 收藏数。 |
| `thumbnail_url` | 缩略图链接，分辨率低于 image_url。 |
| `posted_at` | 发布时间，ISO 8601 格式、UTC 时区（如 2026-08-07T12:34:56+00:00）。 |
| `category` | 分类名称，取值为原平台的分类体系，未做跨平台归一。 |
| `author_name` | 作者昵称（显示名）。 |
| `author_id` | 作者在原平台的唯一 ID。 |
| `author_url` | 作者在原平台的主页链接。 |
| `platform` | 数据来源平台标识，与请求里的 platform 一致（如 xiaohongshu、tiktok）。 |
| `text` | 正文内容，已去除 HTML 标签，全文返回、不截断（个别平台的列表结果本身只有摘要，以该平台的字段说明为准）。 |

实测还返回了 `duration_sec`，接口文档里还没有它的中文说明，表里没列。

完整参数和字段：<https://everyinfra.com/bilibili-api/search?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

#### `video`（视频详情）

| 字段 | 含义 |
|---|---|
| `id` | 该记录在原平台上的唯一标识。 |
| `url` | 该记录在原平台上的可访问链接。 |
| `title` | 标题。 |
| `text` | 正文内容，已去除 HTML 标签，全文返回、不截断（个别平台的列表结果本身只有摘要，以该平台的字段说明为准）。 |
| `duration_seconds` | 时长，单位秒。 |
| `view_count` | 浏览/播放数。 |
| `like_count` | 点赞数。 |
| `comment_count` | 评论数。 |
| `danmaku_count` | 弹幕数。 |
| `favorite_count` | 收藏数。 |
| `coin_count` | 投币数。 |
| `share_count` | 分享/转发数。 |
| `thumbnail_url` | 缩略图链接，分辨率低于 image_url。 |
| `posted_at` | 发布时间，ISO 8601 格式、UTC 时区（如 2026-08-07T12:34:56+00:00）。 |
| `category` | 分类名称，取值为原平台的分类体系，未做跨平台归一。 |
| `platform` | 数据来源平台标识，与请求里的 platform 一致（如 xiaohongshu、tiktok）。 |
| `tags` | 标签数组。 |

实测还返回了 `duration_sec`，接口文档里还没有它的中文说明，表里没列。

完整参数和字段：<https://everyinfra.com/bilibili-api/video?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

#### `comments`（评论）

| 字段 | 含义 |
|---|---|
| `id` | 该记录在原平台上的唯一标识。 |
| `text` | 正文内容，已去除 HTML 标签，全文返回、不截断（个别平台的列表结果本身只有摘要，以该平台的字段说明为准）。 |
| `like_count` | 点赞数。 |
| `reply_count` | 回复数，通常用于评论的子回复。 |
| `author_name` | 作者昵称（显示名）。 |
| `author_id` | 作者在原平台的唯一 ID。 |
| `author_url` | 作者在原平台的主页链接。 |
| `posted_at` | 发布时间，ISO 8601 格式、UTC 时区（如 2026-08-07T12:34:56+00:00）。 |
| `video_id` | 视频 BV 号。 |
| `platform` | 数据来源平台标识，与请求里的 platform 一致（如 xiaohongshu、tiktok）。 |

完整参数和字段：<https://everyinfra.com/bilibili-api/comments?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

#### `profile`（主页或对象详情）

| 字段 | 含义 |
|---|---|
| `user_id` | 用户在原平台的唯一 ID。 |
| `name` | 名称。 |
| `url` | 该记录在原平台上的可访问链接。 |
| `avatar_url` | 头像图片链接。 |
| `gender` | 性别。 |
| `level` | 用户等级（0–6）。 |
| `is_verified` | 是否为平台认证账号（蓝V等）。 |
| `is_vip` | 是否为大会员。 |
| `school` | 学校。 |
| `platform` | 数据来源平台标识，与请求里的 platform 一致（如 xiaohongshu、tiktok）。 |

完整参数和字段：<https://everyinfra.com/bilibili-api/profile?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

#### `user_posts`（用户内容）

| 字段 | 含义 |
|---|---|
| `id` | 该记录在原平台上的唯一标识。 |
| `url` | 该记录在原平台上的可访问链接。 |
| `title` | 标题。 |
| `duration_seconds` | 时长，单位秒。 |
| `view_count` | 浏览/播放数。 |
| `danmaku_count` | 弹幕数。 |
| `thumbnail_url` | 缩略图链接，分辨率低于 image_url。 |
| `posted_at` | 发布时间，ISO 8601 格式、UTC 时区（如 2026-08-07T12:34:56+00:00）。 |
| `category` | 分类名称，取值为原平台的分类体系，未做跨平台归一。 |
| `author_name` | 作者昵称（显示名）。 |
| `author_id` | 作者在原平台的唯一 ID。 |
| `author_url` | 作者在原平台的主页链接。 |
| `platform` | 数据来源平台标识，与请求里的 platform 一致（如 xiaohongshu、tiktok）。 |

实测还返回了 `duration_sec`，接口文档里还没有它的中文说明，表里没列。

完整参数和字段：<https://everyinfra.com/bilibili-api/user_posts?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

### 常见问题

**调用时要不要提供 B站账号或 Cookie？**

不用，请求头带 `Authorization: Bearer` 和 API Key 就能调，请求体写 `platform`、`action` 和能力参数。不用扫码、维护登录状态，也不用开浏览器。

**怎么收费？失败了收不收钱？有免费额度吗？**

按调用次数计费，每千次 $0.56 起。失败、空结果、参数错误和鉴权失败都不收费。新账号有 $1 试用额度，24 小时内有效，不用信用卡，领取方式见定价页 <https://everyinfra.com/pricing?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

**能把 B站接口接进 Claude、Cursor 吗？**

能，远程 MCP 地址是 <https://api.everyinfra.com/mcp>，请求头加 `Authorization: Bearer` 和 API Key，不用装本地程序。Claude、Cursor、ChatGPT、Claude Code 配置见 <https://everyinfra.com/mcp-server?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>。Agent 能自己搜 B站内容、读评论、查账号。

**用 B站数据要注意哪些合规问题？**

EveryInfra 服务条款要求你自行确保数据用法符合适用法律和相关平台条款，禁止用于违法、侵犯隐私或知识产权，或批量抓取用于骚扰、欺诈等恶意用途。用到昵称、头像等个人信息时，只留业务用得上的字段。

**可以拿到 B站的哪些数据？**

B站有 8 个能力：关键词搜索、主页或对象详情、用户内容、视频详情、评论、弹幕、字幕、热门作品。每个能力的参数和返回字段见平台页 <https://everyinfra.com/bilibili-api?utm_source=github&utm_medium=example_repo&utm_campaign=bilibili-api>

### 实测记录

发布前用本仓的代码对线上接口跑过一遍，命令就是「快速开始」里的 5 条。

| 命令 | 时间（新加坡时间） | 返回条数 | 扣费 |
|---|---|---|---|
| `python main.py search 露营装备 --out videos.csv` | 2026-11-21 09:24 | 20 | $0.000556 |
| `python main.py video "<视频链接>" --out video.json` | 2026-11-21 09:24 | 1 | $0.000556 |
| `python main.py comments "<视频链接>" --out comments.csv` | 2026-11-21 09:25 | 10 | $0.000556 |
| `python main.py user <user_id>` | 2026-11-21 09:25 | 1 | $0.000556 |
| `python main.py posts <user_id> --out posts.csv` | 2026-11-21 09:26 | 20 | $0.000556 |

`video` 返回的一条（正文截到 60 字，作者字段没放进来）：

```json
{
  "id": "BV1H44y1t7fU",
  "title": "新手小白第一次露营要买什么装备？看这个视频，看完保证马上退坑 👍",
  "text": "露营要买的装备非常多，坑也多，新手很容易手足无措\n\n今天来跟大家一起盘一盘，一次露营，都需要买些啥\n\n不过，还是建议大家…",
  "duration_seconds": 376,
  "view_count": 500337,
  "like_count": 8799,
  "comment_count": 654,
  "danmaku_count": 777,
  "favorite_count": 10092,
  "coin_count": 2816,
  "share_count": 3379,
  "posted_at": "2021-10-07T04:11:03+00:00",
  "category": "出行",
  "tags": [
    "旅行",
    "生活记录",
    "帐篷",
    "露营",
    "野营",
    "野外",
    "户外",
    "野餐",
    "露营装备"
  ],
  "duration_sec": 376
}
```

## License

MIT
