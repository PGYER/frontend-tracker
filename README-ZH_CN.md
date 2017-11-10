# Frontend Tracker

![GitHub version](https://badge.fury.io/gh/Pgyer%2Ffrontend-tracker.svg)
![Bower version](https://badge.fury.io/bo/frontend-tracker.svg)
![npm version](https://badge.fury.io/js/frontend-tracker.svg)

## 介绍

Frontend Tracker 可以发现前端页面的错误，并且用户察觉错误前将错误发送至指定服务器。

### 特点
1. 记录并发送前端页面产生的错误
1. 记录脚本错误
1. 记录 XHR 请求错误
1. 记录 XHR 请求超时
1. 记录速度较慢的 XHR 请求
1. 记录跨域的 XHR 请求
1. 记录资源加载错误
1. 记录跨域资源加载
1. 正则表达式兼容的 URL 配置方式

## 安装

frontend-tracker 代码可以通过使用使用 Bower

    bower install frontend-tracker --save

或者使用 npm

    bower install frontend-tracker --save

或者直接下载 ZIP 包来获得.

添加到你需要监控错误的页面即可

    <script src="path/to/package/dist/tracker.min.js">

## 配置

添加以下代码到您的代码中以启动 Frontend Tracker

``` html
<script type="text/javascript">
  window.setTracker({
    endpoint: '',
    xhr: {
      log: {
        crossOrigin: true,
        slowRequest: true,
        timeout: true,
        error: true
      },
      origin: [
        'http://www.pgyer.com',
        /.*\.tracup\.com/,
      ],
      timeLimit: {
        send: 0,
        load: 0,
        total: 0
      },
      exclude: []
    },
    resource: {
      log: {
        crossOrigin: true,
        error: true
      },
      origin: [],
      exclude: []
    },
    script: {
      log: {
        error: true
      },
      exclude: []
    }
  })
  </script>
```

### 配置项

#### endpoint

    String
    Required

用于接收错误的 URL ／ URI.

#### xhr

    Object
    Required

用于配置 XHR 错误发生时的行为

| 名称 | 类型 | 功能描述 |
| :-- | :-- | :-- |
| log | Required, Object  | 配置 XHR 错误的记录行为 |
| log.crossOrigin | Required, Boolean, Default: `false` | 当设置成 `true` 时记录跨域的 XHR 请求 |
| log.slowRequest | Required, Boolean, Default: `false` | 当设置成 `true` 时记录速度较慢的 XHR 请求 |
| log.timeout | Required, Boolean, Default: `false` | 当设置成 `true` 时记录超时的 XHR 请求 |
| log.error | Required, Boolean, Default: `false` | 当设置成 `true` 时记录出错的 XHR 请求 |
| origin | Optional, Array | 设置域内的 URI, 支持正则表达式 |
| timeLimit | Optional, Object | 用于描述速度较慢 XHR 请求的时间界定值 |
| timeLimit.send | int, Default: 0 | 请求发送到开始接收数据前的时间 (ms), `0` 表示不限制 |
| timeLimit.load | int, Default: 0 | 响应内容接收的时间 (ms), `0` 表示不限制  |
| timeLimit.total | int, Default: 0 | 请求花费的总时间 (ms), `0` 表示不限制 |
| exclude | Optional, Array | 设置忽略错误的 URI, 支持正则表达式 |

#### resource

    Object
    Required

用于配置资源错误发生时的行为

| 名称 | 类型 | 功能描述 |
| :-- | :-- | :-- |
| log | Required, Object  | 配置资源错误的记录行为 |
| log.crossOrigin | Required, Boolean, Default: `false` | 当设置成 `true` 时记录跨域的资源请求 |
| log.error | Required, Boolean, Default: `false` | 当设置成 `true` 时记录错误的资源请求 |
| origin | Optional, Array | 设置域内的 URI, 支持正则表达式 |
| exclude | Optional, Array | 设置忽略错误的 URI, 支持正则表达式 |

#### script

Object
Required

用于配置脚本错误发生时的行为

| name | type | description |
| :-- | :-- | :-- |
| log | Required, Object  | 配置资源错误的记录行为 |
| log.error | Required, Boolean, Default: `false` | 当设置成 `true` 时记录脚本错误 |
| exclude | Optional, Array | 设置忽略错误脚本文件 URI, 支持正则表达式 |

## 处理错误信息

错误信息会被以 JSON 的形式发送（POST）到设置的 endpoint 上

### 字段

| 名称 | 值 | 描述 |
| :-- | :-- | :-- |
| type | String | 错误类型 `XHR`, `RESOURCE`, `SCRIPT` 的一种|
| data | Object | 详细错误信息 |
| currentURL | string | 发生错误的 URL |
| userAgent | string | 发生错误浏览器的 User-Agent |

错误信息可以通过 `data.message` 获得，详细信息需要通过解析 `data.detail` 来获得

| 类型 | `data.detail` 的结构 | 描述 |
| :-- | :-- | :-- |
| XHR | {request: String, response: {status: int, response: String},timing: {send: int, load: int, total: int}} | `request`: 请求的 URL, `status`: 状态码, `response`: 响应内容, `send`: 发送耗时(ms), `load`: 接收耗时 (ms), `total`: 总耗时 (ms) |
| RESOURCE | {tagname: String, resourceURL: String} |  `tagname`: 标签, `resourceURL`: 资源 URI |
| SCRIPT | {file: String, line: int, column: int, trace: String} | `file`: 脚本文件名, `line`: 行号, `column`: 列号, `trace`: 调用堆栈 |

## 授权方式
Frontend Tracker 以 [GPL-3 licensed](LICENSE) 授权使用.
