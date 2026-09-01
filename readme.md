## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `78834` | `3140` | `83246` |
| **87%** | [Hyper Express](#hyper-express) | `68862` | `3323` | `72323` |
| **46%** | [Node (Default)](#node-default) | `36561` | `10829` | `73175` |
| **42%** | [Fastify](#fastify) | `32802` | `8994` | `50135` |
| **37%** | [Koa](#koa) | `29130` | `14083` | `89670` |
| **37%** | [Hono](#hono) | `28942` | `8198` | `45622` |
| **12%** | [Carbon](#carbon) | `9772` | `2564` | `14032` |
| **9%** | [Express](#express) | `7479` | `1668` | `10385` |


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
    Reqs/sec     10572.58    8519.71   88085.57
    Latency        4.72ms     4.30ms   368.65ms
    HTTP codes:
      1xx - 0, 2xx - 88562, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11438
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11438
    Throughput:     2.13MB/s
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
    Reqs/sec      8409.36    6568.46   82564.82
    Latency        5.93ms     3.84ms   352.89ms
    HTTP codes:
      1xx - 0, 2xx - 90760, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9240
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9240
    Throughput:     2.19MB/s
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
    Reqs/sec     34226.17    8987.29   51125.82
    Latency        1.46ms     1.85ms   163.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.76MB/s
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
    Reqs/sec     31772.44    9686.02   46633.41
    Latency        1.57ms     2.01ms   179.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.17MB/s
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
    Reqs/sec     69985.10    3613.79   73461.57
    Latency      712.71us    73.03us     4.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.94MB/s
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
    Reqs/sec     28080.14   11797.73   76005.70
    Latency        1.78ms     2.31ms   198.28ms
    HTTP codes:
      1xx - 0, 2xx - 92344, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7656
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7656
    Throughput:     5.85MB/s
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
    Reqs/sec     35101.18    8921.38   75370.25
    Latency        1.42ms     1.74ms   142.97ms
    HTTP codes:
      1xx - 0, 2xx - 96341, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3659
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3659
    Throughput:     7.74MB/s
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
    Reqs/sec     78334.89    1934.62   82709.32
    Latency      635.65us   160.15us     8.46ms
    HTTP codes:
      1xx - 0, 2xx - 96635, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3365
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3365
    Throughput:    11.98MB/s
  ```


