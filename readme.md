## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75923` | `3057` | `83289` |
| **84%** | [Hyper Express](#hyper-express) | `63623` | `3572` | `66495` |
| **34%** | [Node (Default)](#node-default) | `25458` | `8177` | `60480` |
| **32%** | [Fastify](#fastify) | `24190` | `7559` | `35670` |
| **26%** | [Hono](#hono) | `20069` | `5847` | `29083` |
| **25%** | [Koa](#koa) | `19010` | `9040` | `79293` |
| **11%** | [Carbon](#carbon) | `8291` | `1439` | `10337` |
| **8%** | [Express](#express) | `6438` | `1066` | `8283` |


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
    Reqs/sec      8767.19    5611.26   77547.66
    Latency        5.69ms     4.37ms   374.89ms
    HTTP codes:
      1xx - 0, 2xx - 92577, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7423
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7423
    Throughput:     1.85MB/s
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
    Reqs/sec      6813.52    4915.17   64646.22
    Latency        7.32ms     4.03ms   368.10ms
    HTTP codes:
      1xx - 0, 2xx - 91808, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8192
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8192
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
    Reqs/sec     22970.47    6433.40   35691.78
    Latency        2.18ms     1.95ms   176.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.20MB/s
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
    Reqs/sec     20299.98    5373.52   29084.51
    Latency        2.46ms     2.03ms   184.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     62637.35    3219.06   65488.49
    Latency      795.93us    67.09us     3.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     18953.16    8745.80   80721.65
    Latency        2.64ms     2.51ms   219.00ms
    HTTP codes:
      1xx - 0, 2xx - 92098, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7902
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7902
    Throughput:     3.94MB/s
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
    Reqs/sec     26770.97   10083.36   80951.10
    Latency        1.86ms     1.98ms   168.05ms
    HTTP codes:
      1xx - 0, 2xx - 94185, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5815
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5815
    Throughput:     5.78MB/s
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
    Reqs/sec     75082.58    3352.33   85348.97
    Latency      662.86us   149.63us     5.43ms
    HTTP codes:
      1xx - 0, 2xx - 97193, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2807
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2807
    Throughput:    11.55MB/s
  ```


