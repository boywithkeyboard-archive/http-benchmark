## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75275` | `2203` | `84152` |
| **84%** | [Hyper Express](#hyper-express) | `63572` | `3409` | `67206` |
| **32%** | [Node (Default)](#node-default) | `24445` | `7327` | `60931` |
| **30%** | [Fastify](#fastify) | `22865` | `6800` | `35384` |
| **28%** | [Hono](#hono) | `20902` | `6208` | `30795` |
| **26%** | [Koa](#koa) | `19196` | `8215` | `66123` |
| **11%** | [Carbon](#carbon) | `8257` | `1457` | `10428` |
| **8%** | [Express](#express) | `6378` | `1050` | `8354` |


### In Detail

- #### Carbon
  [NPM](https://npmjs.com/@sinclair/carbon) | [GitHub](https://github.com/sinclairzx81/carbon)
  ```js
  import { listen } from '@sinclair/carbon/http'

  listen({
    hostname: '127.0.0.1',
    port: 3000
  }, () => {
    return new Response('Hello World', {
      status: 200,
      headers: {
        'content-type': 'text/plain'
      }
    })
  })
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec      8740.95    5559.23   70804.70
    Latency        5.71ms     4.28ms   372.92ms
    HTTP codes:
      1xx - 0, 2xx - 92402, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7598
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7598
    Throughput:     1.83MB/s
  ```

- #### Express
  [NPM](https://npmjs.com/express) | [GitHub](https://github.com/expressjs/express)
  ```js
  import express from 'express'

  const app = express()

  app.get('/', function (req, res) {
    res.send('Hello World')
  })

  app.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec      6525.98    1113.52    8437.60
    Latency        7.66ms     3.65ms   351.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
  ```

- #### Fastify
  [NPM](https://npmjs.com/fastify) | [GitHub](https://github.com/fastify/fastify)
  ```js
  import fastify from 'fastify'

  const app = fastify({
    logger: false
  })

  app.get('/', (req, res) => {
    res.send('Hello World')
  })

  app.listen({ port: 3000 }, (err) => {
    if (err) throw err
  })
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     25077.88    7825.97   36226.57
    Latency        1.99ms     2.09ms   182.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.69MB/s
  ```

- #### Hono
  [NPM](https://npmjs.com/hono) | [GitHub](https://github.com/honojs/hono)
  ```js
  import { serve } from '@hono/node-server'
  import { Hono } from 'hono'

  const app = new Hono()

  app.get('/', (c) => c.text('Hello World'))

  serve(app)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     20705.73    5960.24   30906.37
    Latency        2.41ms     1.99ms   179.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
  ```

- #### Hyper Express
  [NPM](https://npmjs.com/hyper-express) | [GitHub](https://github.com/kartikk221/hyper-express)
  ```js
  import HyperExpress from 'hyper-express'

  const server = new HyperExpress.Server()

  server.get('/', (req, res) => {
    res.send('Hello World')
  })

  server.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     63547.94    3593.48   66203.96
    Latency      784.55us    67.74us     2.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.03MB/s
  ```

- #### Koa
  [NPM](https://npmjs.com/koa) | [GitHub](https://github.com/koajs/koa)
  ```js
  import Koa from 'koa'

  const app = new Koa()

  app.use(ctx => {
    ctx.body = 'Hello World'
  })

  app.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     18930.27    8042.63   66625.46
    Latency        2.63ms     2.33ms   204.41ms
    HTTP codes:
      1xx - 0, 2xx - 92984, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7016
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7016
    Throughput:     3.98MB/s
  ```

- #### Node (Default)
  [Website](https://nodejs.org/api/http.html)
  ```js
  import { createServer } from 'node:http'

  const server = createServer((req, res) => {
    res.writeHead(200, {
      'content-type': 'text/plain'
    })

    res.write('Hello World')

    res.end()
  })

  server.listen(3000, '127.0.0.1')
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     25648.73    8345.01   78624.03
    Latency        1.94ms     1.80ms   154.67ms
    HTTP codes:
      1xx - 0, 2xx - 96406, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3594
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3594
    Throughput:     5.66MB/s
  ```

- #### uWS
  [GitHub](https://github.com/uNetworking/uWebSockets.js)
  ```js
  import { App } from 'uWebSockets.js'

  const app = App()

  app.get('/', (res, req) => {
    res.end('Hello World')
  })

  app.listen(3000, () => {})
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     75370.09    2015.37   78750.59
    Latency      660.84us   135.63us     6.96ms
    HTTP codes:
      1xx - 0, 2xx - 96929, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3071
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3071
    Throughput:    11.57MB/s
  ```


