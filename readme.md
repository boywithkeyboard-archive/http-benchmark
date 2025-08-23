## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76747` | `3432` | `85333` |
| **82%** | [Hyper Express](#hyper-express) | `63085` | `3770` | `67205` |
| **36%** | [Node (Default)](#node-default) | `27352` | `8879` | `76104` |
| **30%** | [Fastify](#fastify) | `23273` | `6485` | `37344` |
| **28%** | [Hono](#hono) | `21428` | `6146` | `30652` |
| **26%** | [Koa](#koa) | `19931` | `8987` | `81984` |
| **11%** | [Carbon](#carbon) | `8280` | `1424` | `10766` |
| **8%** | [Express](#express) | `6382` | `1011` | `8701` |


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
    Reqs/sec      8735.30    5708.90   66239.88
    Latency        5.72ms     4.27ms   368.34ms
    HTTP codes:
      1xx - 0, 2xx - 92336, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7664
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7664
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
    Reqs/sec      7231.43    5764.59   78184.00
    Latency        6.90ms     3.82ms   352.26ms
    HTTP codes:
      1xx - 0, 2xx - 90775, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9225
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9225
    Throughput:     1.88MB/s
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
    Reqs/sec     25134.63    7610.70   38217.86
    Latency        1.99ms     1.89ms   172.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.70MB/s
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
    Reqs/sec     21353.05    5942.17   31370.82
    Latency        2.34ms     2.05ms   184.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     63551.09    3476.10   69711.20
    Latency      784.44us    63.41us     2.59ms
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
    Reqs/sec     19523.79    9363.40   79063.38
    Latency        2.56ms     2.37ms   209.24ms
    HTTP codes:
      1xx - 0, 2xx - 91202, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8798
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8798
    Throughput:     4.03MB/s
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
    Reqs/sec     26879.76    8728.23   76757.29
    Latency        1.86ms     1.87ms   157.06ms
    HTTP codes:
      1xx - 0, 2xx - 96473, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3527
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3527
    Throughput:     5.94MB/s
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
    Reqs/sec     74990.37    2843.46   80473.57
    Latency      664.70us   128.86us     6.23ms
    HTTP codes:
      1xx - 0, 2xx - 97469, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2531
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2531
    Throughput:    11.56MB/s
  ```


