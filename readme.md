## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73663` | `4615` | `83739` |
| **86%** | [Hyper Express](#hyper-express) | `63333` | `4034` | `73686` |
| **36%** | [Node (Default)](#node-default) | `26742` | `7970` | `41882` |
| **33%** | [Fastify](#fastify) | `24602` | `7201` | `37931` |
| **29%** | [Hono](#hono) | `21286` | `5975` | `30203` |
| **26%** | [Koa](#koa) | `19106` | `7007` | `59535` |
| **11%** | [Carbon](#carbon) | `8385` | `1431` | `10486` |
| **9%** | [Express](#express) | `6490` | `1044` | `8350` |


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
    Reqs/sec      8856.82    4894.07   66627.86
    Latency        5.63ms     4.28ms   368.91ms
    HTTP codes:
      1xx - 0, 2xx - 92828, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7172
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7172
    Throughput:     1.87MB/s
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
    Reqs/sec      6914.79    4009.60   51146.95
    Latency        7.22ms     3.84ms   353.60ms
    HTTP codes:
      1xx - 0, 2xx - 93359, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6641
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6641
    Throughput:     1.85MB/s
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
    Reqs/sec     25182.16    8261.54   37393.93
    Latency        1.98ms     1.96ms   178.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.71MB/s
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
    Reqs/sec     21016.63    5632.11   29921.16
    Latency        2.38ms     1.94ms   175.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     62052.75    3726.41   70778.45
    Latency      803.57us    71.81us     4.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     19581.30    7107.42   56411.97
    Latency        2.55ms     2.45ms   213.49ms
    HTTP codes:
      1xx - 0, 2xx - 94213, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5787
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5787
    Throughput:     4.17MB/s
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
    Reqs/sec     26491.34    7405.68   62810.98
    Latency        1.88ms     1.89ms   161.21ms
    HTTP codes:
      1xx - 0, 2xx - 97214, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2786
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2786
    Throughput:     5.90MB/s
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
    Reqs/sec     74429.13    5431.47   87613.04
    Latency      669.86us   167.85us     6.20ms
    HTTP codes:
      1xx - 0, 2xx - 97146, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2854
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2854
    Throughput:    11.43MB/s
  ```


