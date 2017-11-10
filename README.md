# Frontend Tracker

![GitHub version](https://badge.fury.io/gh/Pgyer%2Ffrontend-tracker.svg)
![Bower version](https://badge.fury.io/bo/frontend-tracker.svg)
![npm version](https://badge.fury.io/js/frontend-tracker.svg)


## Intro to Frontend Tracker

Sending frontend error to server. Get frontend error before Issue created.

### Highlight
1. Get frontend error when occurred
1. Logging script error
1. Logging XHR request error
1. Logging XHR request timeout
1. Logging XHR slow request
1. Logging cross origin XHR request
1. Logging resource loading error
1. Logging cross origin resource loading
1. Config URL with regular expression

## Installation

Use Bower

    bower install frontend-tracker --save

or Use npm

    bower install frontend-tracker --save

or directly download the ZIP archive to get frontend-tracker.

then add into the page you want to inspect errors.

    <script src="path/to/package/dist/tracker.min.js">

## Configuration

Add following code into your html file to start and config Frontend Tracker.

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

### Options

#### endpoint

    String
    Required

URL or URI to post error message.

#### xhr

    Object
    Required

An object to config when XHR error occurred.

| name | type | description |
| :-- | :-- | :-- |
| log | Required, Object  | Object to config log capture |
| log.crossOrigin | Required, Boolean, Default: `false` | Capture XHR cross origin if set to `true` |
| log.slowRequest | Required, Boolean, Default: `false` | Capture slow request if set to `true` |
| log.timeout | Required, Boolean, Default: `false` | Capture XHR timeout event if set to `true` |
| log.error | Required, Boolean, Default: `false` | Capture XHR error event if set to `true` |
| origin | Optional, Array | Set Origin, both Sting and Regular expression accepted |
| timeLimit | Optional, Object | A object to describe slow log timing threshold. |
| timeLimit.send | int, Default: 0 | XHR sending time threshold. `0` is no limit (ms) |
| timeLimit.load | int, Default: 0 | XHR loading time threshold. `0` is no limit  (ms) |
| timeLimit.total | int, Default: 0 | XHR total request time threshold. `0` is no limit (ms) |
| exclude | Optional, Array | Set ignore path, both Sting and Regular expression accepted |

#### resource

    Object
    Required

An object to config when resource error occurred.

| name | type | description |
| :-- | :-- | :-- |
| log | Required, Object  | Object to config log capture |
| log.crossOrigin | Required, Boolean, Default: `false` | Capture resource cross origin if set to `true` |
| log.error | Required, Boolean, Default: `false` | Capture resource error event if set to `true` |
| origin | Optional, Array | Set Origin, both Sting and Regular expression accepted |
| exclude | Optional, Array | Set ignore path, both Sting and Regular expression accepted |

#### script

Object
Required

An object to config when script error occurred.

| name | type | description |
| :-- | :-- | :-- |
| log | Required, Object  | Object to config log capture |
| log.error | Required, Boolean, Default: `false` | Capture script error event if set to `true` |
| exclude | Optional, Array | Set ignore filename, both Sting and Regular expression accepted |

## Handling Error Message

An error message in JSON format will post to endpoint when errors occurred.

### Fields

| name | value | description |
| :-- | :-- | :-- |
| type | String | Type of error message. one of `XHR`, `RESOURCE`, `SCRIPT` |
| data | Object | Detailed error information. |
| currentURL | String | URL of target page. |
| userAgent | String | User-Agent String of target Browser. |

Error message can be get from `data.message`. Detail error data can be get via parsing from `data.detail` field.

| type | Structure of `data.detail` | description |
| :-- | :-- | :-- |
| XHR | {request: String, response: {status: int, response: String},timing: {send: int, load: int, total: int}} | `request`: Request URL, `status`: status code of xhr, `response`: response text form xhr, `send`: sending time of xhr (ms), `load`: loading time of xhr (ms), `total`: request time of xhr (ms) |
| RESOURCE | {tagname: String, resourceURL: String} |  `tagname`: tagname of element, `resourceURL`: URL of resource |
| SCRIPT | {file: String, line: int, column: int, trace: String} | `file`: script filename, `line`: line number of error script, `column`: column number of error script, `trace`: stack trace of error |



## License
Frontend Tracker is [GPL-3 licensed](LICENSE).
