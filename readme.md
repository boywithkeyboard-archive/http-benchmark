## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `173132` | `10842` | `188656` |
| **87%** | [Hyper Express](#hyper-express) | `151247` | `9760` | `162023` |
| **37%** | [Node (Default)](#node-default) | `64660` | `14613` | `149007` |
| **33%** | [Fastify](#fastify) | `56887` | `11393` | `67902` |
| **30%** | [Koa](#koa) | `52652` | `24508` | `167226` |
| **28%** | [Hono](#hono) | `48700` | `8812` | `54401` |
| **12%** | [Carbon](#carbon) | `20671` | `4805` | `30341` |
| **8%** | [Express](#express) | `14139` | `2395` | `20406` |


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
    Reqs/sec     23033.36   13652.52  138788.14
    Latency        2.16ms     2.84ms   232.06ms
    HTTP codes:
      1xx - 0, 2xx - 90872, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9128
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9128
    Throughput:     4.76MB/s
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
    Reqs/sec     17328.71   18559.57  164291.97
    Latency        2.88ms     2.14ms   192.76ms
    HTTP codes:
      1xx - 0, 2xx - 84330, 3xx - 0, 4xx - 0, 5xx - 0
      others - 15670
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 15670
    Throughput:     4.18MB/s
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
    Reqs/sec     67851.76   27264.16  160650.97
    Latency      733.14us     0.93ms    70.36ms
    HTTP codes:
      1xx - 0, 2xx - 78296, 3xx - 0, 4xx - 0, 5xx - 0
      others - 21704
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 21704
    Throughput:    12.07MB/s
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
    Reqs/sec     50971.88   21501.86  170642.73
    Latency        0.98ms     1.10ms    75.05ms
    HTTP codes:
      1xx - 0, 2xx - 89092, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10908
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10908
    Throughput:    10.27MB/s
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
    Reqs/sec    149484.96   10020.01  160948.02
    Latency      332.57us   169.49us     8.17ms
    HTTP codes:
      1xx - 0, 2xx - 91058, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8942
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8942
    Throughput:    19.32MB/s
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
    Reqs/sec     46932.35   18394.54  133181.82
    Latency        1.06ms     1.18ms    85.94ms
    HTTP codes:
      1xx - 0, 2xx - 90370, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9630
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9630
    Throughput:     9.61MB/s
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
    Reqs/sec     64647.20   14530.10  146508.76
    Latency      770.64us   768.32us    52.52ms
    HTTP codes:
      1xx - 0, 2xx - 96120, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3880
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3880
    Throughput:    14.23MB/s
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
    Reqs/sec    174687.83   11256.05  187902.08
    Latency      284.27us   113.01us     6.22ms
    HTTP codes:
      1xx - 0, 2xx - 95351, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4649
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4649
    Throughput:    26.35MB/s
  ```


