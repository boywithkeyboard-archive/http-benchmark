## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75056` | `5572` | `87136` |
| **84%** | [Hyper Express](#hyper-express) | `63066` | `4112` | `68048` |
| **37%** | [Node (Default)](#node-default) | `27903` | `8409` | `60005` |
| **33%** | [Fastify](#fastify) | `24510` | `7159` | `36753` |
| **27%** | [Hono](#hono) | `20607` | `5734` | `29897` |
| **25%** | [Koa](#koa) | `18632` | `6271` | `48641` |
| **11%** | [Carbon](#carbon) | `8316` | `1417` | `10389` |
| **9%** | [Express](#express) | `6569` | `1087` | `8548` |


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
    Reqs/sec      8858.78    3931.20   51946.45
    Latency        5.64ms     4.28ms   372.75ms
    HTTP codes:
      1xx - 0, 2xx - 94839, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5161
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5161
    Throughput:     1.91MB/s
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
    Reqs/sec      7097.51    4175.71   48501.44
    Latency        7.03ms     3.88ms   353.68ms
    HTTP codes:
      1xx - 0, 2xx - 92814, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7186
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7174
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 12
    Throughput:     1.89MB/s
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
    Reqs/sec     24655.43    7545.28   37378.17
    Latency        2.03ms     2.09ms   186.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.59MB/s
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
    Reqs/sec     21743.13    6140.33   30663.99
    Latency        2.30ms     2.07ms   182.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.91MB/s
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
    Reqs/sec     61698.46    3496.17   64096.17
    Latency      808.16us    72.89us     3.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.76MB/s
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
    Reqs/sec     19081.08    7423.32   60514.22
    Latency        2.62ms     2.35ms   204.76ms
    HTTP codes:
      1xx - 0, 2xx - 92417, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7583
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7583
    Throughput:     3.99MB/s
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
    Reqs/sec     27733.08    7673.01   52802.75
    Latency        1.80ms     1.84ms   157.70ms
    HTTP codes:
      1xx - 0, 2xx - 97418, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2582
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2582
    Throughput:     6.18MB/s
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
    Reqs/sec     73586.12    5214.42   84975.68
    Latency      677.29us   157.82us     8.86ms
    HTTP codes:
      1xx - 0, 2xx - 98015, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1985
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1985
    Throughput:    11.41MB/s
  ```


