## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75175` | `3066` | `83466` |
| **84%** | [Hyper Express](#hyper-express) | `63076` | `4153` | `69149` |
| **34%** | [Node (Default)](#node-default) | `25573` | `8214` | `67673` |
| **31%** | [Fastify](#fastify) | `23521` | `7287` | `36640` |
| **29%** | [Hono](#hono) | `21943` | `6271` | `31040` |
| **26%** | [Koa](#koa) | `19784` | `8807` | `70982` |
| **11%** | [Carbon](#carbon) | `8037` | `1418` | `10366` |
| **8%** | [Express](#express) | `6336` | `1052` | `8509` |


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
    Reqs/sec      8995.83    6785.06   76543.72
    Latency        5.55ms     4.28ms   368.92ms
    HTTP codes:
      1xx - 0, 2xx - 89861, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10139
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10139
    Throughput:     1.84MB/s
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
    Reqs/sec      7095.20    6792.40   82276.83
    Latency        7.04ms     3.91ms   360.20ms
    HTTP codes:
      1xx - 0, 2xx - 88027, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11973
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11973
    Throughput:     1.79MB/s
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
    Reqs/sec     24615.74    7883.66   36517.74
    Latency        2.03ms     2.05ms   185.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.58MB/s
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
    Reqs/sec     21143.91    6596.12   30647.95
    Latency        2.36ms     2.03ms   185.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     63122.08    3673.40   66700.06
    Latency      790.41us    73.96us     3.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.96MB/s
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
    Reqs/sec     18978.15    8886.96   81057.85
    Latency        2.63ms     2.53ms   221.07ms
    HTTP codes:
      1xx - 0, 2xx - 91334, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8666
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8666
    Throughput:     3.92MB/s
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
    Reqs/sec     25725.22    8496.09   64246.61
    Latency        1.94ms     1.98ms   170.04ms
    HTTP codes:
      1xx - 0, 2xx - 97420, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2580
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2580
    Throughput:     5.74MB/s
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
    Reqs/sec     76425.56    3286.20   85601.22
    Latency      651.20us   205.04us    11.51ms
    HTTP codes:
      1xx - 0, 2xx - 96372, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3628
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3628
    Throughput:    11.65MB/s
  ```


