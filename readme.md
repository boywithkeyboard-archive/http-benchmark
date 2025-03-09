## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74965` | `6690` | `103190` |
| **83%** | [Hyper Express](#hyper-express) | `62550` | `2642` | `65229` |
| **34%** | [Node (Default)](#node-default) | `25760` | `7184` | `57606` |
| **32%** | [Fastify](#fastify) | `24075` | `7027` | `36568` |
| **29%** | [Hono](#hono) | `21480` | `6081` | `29800` |
| **26%** | [Koa](#koa) | `19231` | `7541` | `66522` |
| **11%** | [Carbon](#carbon) | `8318` | `1392` | `10469` |
| **9%** | [Express](#express) | `6420` | `1031` | `8516` |


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
    Reqs/sec      8795.44    4097.49   51426.17
    Latency        5.67ms     4.34ms   375.47ms
    HTTP codes:
      1xx - 0, 2xx - 94549, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5451
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5451
    Throughput:     1.89MB/s
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
    Reqs/sec      7054.66    4958.41   54594.58
    Latency        7.07ms     3.93ms   356.13ms
    HTTP codes:
      1xx - 0, 2xx - 90923, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9077
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9077
    Throughput:     1.84MB/s
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
    Reqs/sec     24971.28    7629.28   36925.88
    Latency        2.00ms     2.03ms   183.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.67MB/s
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
    Reqs/sec     21220.35    5775.03   30273.25
    Latency        2.35ms     1.93ms   175.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     62517.62    3531.91   67408.66
    Latency      797.57us    68.69us     3.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     18935.10    6776.58   53708.35
    Latency        2.64ms     2.33ms   202.46ms
    HTTP codes:
      1xx - 0, 2xx - 93716, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6284
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6284
    Throughput:     4.01MB/s
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
    Reqs/sec     26675.83    7793.08   52035.85
    Latency        1.87ms     1.90ms   161.64ms
    HTTP codes:
      1xx - 0, 2xx - 97762, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2238
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2238
    Throughput:     5.97MB/s
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
    Reqs/sec     75126.87    6165.45   90418.46
    Latency      663.18us   159.42us     6.66ms
    HTTP codes:
      1xx - 0, 2xx - 97984, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2016
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2016
    Throughput:    11.65MB/s
  ```


