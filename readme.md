## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73851` | `4932` | `82802` |
| **86%** | [Hyper Express](#hyper-express) | `63665` | `5198` | `96519` |
| **35%** | [Node (Default)](#node-default) | `25560` | `7109` | `55254` |
| **34%** | [Fastify](#fastify) | `24970` | `7613` | `38249` |
| **29%** | [Hono](#hono) | `21409` | `6345` | `30887` |
| **25%** | [Koa](#koa) | `18132` | `6597` | `53068` |
| **11%** | [Carbon](#carbon) | `8240` | `1410` | `10319` |
| **9%** | [Express](#express) | `6480` | `1056` | `8449` |


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
    Reqs/sec      8838.65    5136.06   63967.65
    Latency        5.64ms     4.29ms   368.28ms
    HTTP codes:
      1xx - 0, 2xx - 92385, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7615
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7615
    Throughput:     1.86MB/s
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
    Reqs/sec      6571.37    1108.64    8505.88
    Latency        7.61ms     3.62ms   351.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24185.85    7596.14   37790.03
    Latency        2.07ms     1.99ms   177.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     21050.06    6098.40   30601.94
    Latency        2.37ms     2.00ms   181.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     62852.24    3469.23   67701.08
    Latency      793.37us    78.36us     3.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     18679.61    6886.69   57427.07
    Latency        2.67ms     2.41ms   211.62ms
    HTTP codes:
      1xx - 0, 2xx - 94168, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5832
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5832
    Throughput:     3.98MB/s
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
    Reqs/sec     25622.17    7285.86   47488.84
    Latency        1.95ms     1.81ms   156.09ms
    HTTP codes:
      1xx - 0, 2xx - 98102, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1898
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1898
    Throughput:     5.76MB/s
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
    Reqs/sec     73734.72    4549.06   81231.40
    Latency      675.94us   158.90us     6.24ms
    HTTP codes:
      1xx - 0, 2xx - 97248, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2752
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2752
    Throughput:    11.35MB/s
  ```


