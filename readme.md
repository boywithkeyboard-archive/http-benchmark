## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73349` | `5585` | `85512` |
| **84%** | [Hyper Express](#hyper-express) | `61318` | `5191` | `92869` |
| **34%** | [Node (Default)](#node-default) | `24997` | `6784` | `43856` |
| **33%** | [Fastify](#fastify) | `24484` | `7548` | `35358` |
| **28%** | [Hono](#hono) | `20372` | `5263` | `29085` |
| **25%** | [Koa](#koa) | `18597` | `7112` | `58627` |
| **11%** | [Carbon](#carbon) | `8042` | `1389` | `10062` |
| **9%** | [Express](#express) | `6274` | `1069` | `8370` |


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
    Reqs/sec      8565.10    4159.80   59201.51
    Latency        5.83ms     4.36ms   377.07ms
    HTTP codes:
      1xx - 0, 2xx - 94418, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5582
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5582
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
    Reqs/sec      6390.63    1083.45    8429.98
    Latency        7.82ms     3.86ms   365.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     24962.20    7715.39   35765.38
    Latency        2.00ms     2.05ms   186.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.66MB/s
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
    Reqs/sec     20598.06    5581.49   28853.52
    Latency        2.42ms     2.07ms   186.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     60362.92    3038.59   77889.65
    Latency      828.83us    69.59us     3.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.55MB/s
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
    Reqs/sec     19058.72    7712.03   62613.29
    Latency        2.62ms     2.42ms   210.60ms
    HTTP codes:
      1xx - 0, 2xx - 91617, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8383
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8383
    Throughput:     3.95MB/s
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
    Reqs/sec     25679.95    7795.17   56847.82
    Latency        1.94ms     1.85ms   160.32ms
    HTTP codes:
      1xx - 0, 2xx - 96796, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3204
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3204
    Throughput:     5.69MB/s
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
    Reqs/sec     71920.91    5251.95   82937.00
    Latency      692.19us   202.19us    10.19ms
    HTTP codes:
      1xx - 0, 2xx - 95043, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4957
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4957
    Throughput:    10.82MB/s
  ```


