## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74916` | `2669` | `79190` |
| **82%** | [Hyper Express](#hyper-express) | `61751` | `4040` | `67155` |
| **33%** | [Fastify](#fastify) | `24594` | `7902` | `34506` |
| **30%** | [Node (Default)](#node-default) | `22743` | `6822` | `70048` |
| **27%** | [Hono](#hono) | `20013` | `5896` | `28417` |
| **25%** | [Koa](#koa) | `18590` | `7775` | `63012` |
| **11%** | [Carbon](#carbon) | `8067` | `1437` | `10119` |
| **8%** | [Express](#express) | `6252` | `1063` | `8268` |


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
    Reqs/sec      8652.78    4872.01   58666.45
    Latency        5.77ms     4.53ms   388.76ms
    HTTP codes:
      1xx - 0, 2xx - 93455, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6545
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6545
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
    Reqs/sec      6152.86     989.48    8150.14
    Latency        8.12ms     3.83ms   368.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     23945.75    7415.20   35261.64
    Latency        2.09ms     2.14ms   191.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.43MB/s
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
    Reqs/sec     20862.92    6089.94   29302.58
    Latency        2.39ms     2.07ms   189.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     62257.06    3414.00   65175.96
    Latency      801.10us    65.13us     2.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18663.08    8432.85   70695.55
    Latency        2.67ms     2.44ms   213.23ms
    HTTP codes:
      1xx - 0, 2xx - 91957, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8043
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8043
    Throughput:     3.88MB/s
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
    Reqs/sec     24972.90    7899.43   64347.94
    Latency        2.00ms     1.98ms   176.42ms
    HTTP codes:
      1xx - 0, 2xx - 97081, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2919
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2919
    Throughput:     5.55MB/s
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
    Reqs/sec     74393.71    2506.22   82444.80
    Latency      669.21us   136.65us     6.32ms
    HTTP codes:
      1xx - 0, 2xx - 96247, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3753
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3753
    Throughput:    11.33MB/s
  ```


