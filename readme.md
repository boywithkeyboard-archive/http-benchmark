## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `81019` | `3292` | `86955` |
| **87%** | [Hyper Express](#hyper-express) | `70588` | `3292` | `73843` |
| **44%** | [Fastify](#fastify) | `35716` | `12646` | `52583` |
| **44%** | [Node (Default)](#node-default) | `35457` | `10103` | `90849` |
| **36%** | [Hono](#hono) | `29484` | `8321` | `45189` |
| **35%** | [Koa](#koa) | `28746` | `12711` | `83144` |
| **12%** | [Carbon](#carbon) | `9682` | `2488` | `13591` |
| **9%** | [Express](#express) | `7501` | `1746` | `10727` |


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
    Reqs/sec     10247.19    8206.13   81195.94
    Latency        4.87ms     4.47ms   391.09ms
    HTTP codes:
      1xx - 0, 2xx - 89116, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10884
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10884
    Throughput:     2.07MB/s
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
    Reqs/sec      8146.34    6943.42   77994.46
    Latency        6.13ms     3.74ms   340.98ms
    HTTP codes:
      1xx - 0, 2xx - 89719, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10281
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10281
    Throughput:     2.09MB/s
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
    Reqs/sec     33909.36   10775.05   52842.18
    Latency        1.47ms     1.86ms   166.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.68MB/s
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
    Reqs/sec     30737.45   10135.94   45019.96
    Latency        1.62ms     2.16ms   193.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.95MB/s
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
    Reqs/sec     70035.99    3325.02   73490.44
    Latency      712.38us    63.64us     2.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.95MB/s
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
    Reqs/sec     28431.74   11457.00   77475.06
    Latency        1.76ms     2.28ms   192.96ms
    HTTP codes:
      1xx - 0, 2xx - 92694, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7306
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7306
    Throughput:     5.95MB/s
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
    Reqs/sec     35323.37   10039.05   73483.52
    Latency        1.41ms     1.81ms   152.57ms
    HTTP codes:
      1xx - 0, 2xx - 97105, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2895
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2895
    Throughput:     7.85MB/s
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
    Reqs/sec     82258.56    3505.12   88128.39
    Latency      604.70us   172.73us    12.60ms
    HTTP codes:
      1xx - 0, 2xx - 94495, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5505
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5505
    Throughput:    12.30MB/s
  ```


