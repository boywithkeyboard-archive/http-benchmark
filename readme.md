## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `160986` | `6287` | `175890` |
| **80%** | [Hyper Express](#hyper-express) | `129115` | `7646` | `139861` |
| **27%** | [Node (Default)](#node-default) | `43931` | `9499` | `124272` |
| **25%** | [Fastify](#fastify) | `40585` | `6008` | `46989` |
| **25%** | [Koa](#koa) | `40037` | `19184` | `163021` |
| **24%** | [Hono](#hono) | `38203` | `6160` | `50546` |
| **12%** | [Carbon](#carbon) | `19199` | `4446` | `27422` |
| **8%** | [Express](#express) | `13257` | `2308` | `18008` |


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
    Reqs/sec     21081.78   15391.17  142595.22
    Latency        2.36ms     3.05ms   247.74ms
    HTTP codes:
      1xx - 0, 2xx - 88836, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11164
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11164
    Throughput:     4.27MB/s
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
    Reqs/sec     16303.16   19368.77  169218.81
    Latency        3.06ms     2.40ms   215.47ms
    HTTP codes:
      1xx - 0, 2xx - 81560, 3xx - 0, 4xx - 0, 5xx - 0
      others - 18440
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 18440
    Throughput:     3.81MB/s
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
    Reqs/sec     46639.76   23802.67  150315.71
    Latency        1.07ms     1.09ms    87.15ms
    HTTP codes:
      1xx - 0, 2xx - 79244, 3xx - 0, 4xx - 0, 5xx - 0
      others - 20756
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 20753
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 3
    Throughput:     8.41MB/s
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
    Reqs/sec     41379.52   19600.24  147406.77
    Latency        1.20ms     1.12ms    84.52ms
    HTTP codes:
      1xx - 0, 2xx - 88623, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11377
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11377
    Throughput:     8.30MB/s
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
    Reqs/sec    130210.92    8420.55  153731.50
    Latency      381.01us   234.69us    10.53ms
    HTTP codes:
      1xx - 0, 2xx - 88762, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11238
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11238
    Throughput:    16.42MB/s
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
    Reqs/sec     41046.81   20642.10  160451.57
    Latency        1.21ms     1.13ms    93.26ms
    HTTP codes:
      1xx - 0, 2xx - 87115, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12885
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12885
    Throughput:     8.10MB/s
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
    Reqs/sec     44996.46   11531.68  143468.72
    Latency        1.11ms     1.00ms    81.38ms
    HTTP codes:
      1xx - 0, 2xx - 95503, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4497
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4497
    Throughput:     9.83MB/s
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
    Reqs/sec    156519.51    6290.61  170576.49
    Latency      316.19us   122.46us     4.62ms
    HTTP codes:
      1xx - 0, 2xx - 92211, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7789
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7789
    Throughput:    22.83MB/s
  ```


