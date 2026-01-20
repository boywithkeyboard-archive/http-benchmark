## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73540` | `2688` | `79857` |
| **82%** | [Hyper Express](#hyper-express) | `60612` | `3470` | `62655` |
| **32%** | [Node (Default)](#node-default) | `23178` | `7217` | `70724` |
| **31%** | [Fastify](#fastify) | `23054` | `7384` | `36322` |
| **28%** | [Hono](#hono) | `20320` | `6084` | `29489` |
| **24%** | [Koa](#koa) | `18002` | `8322` | `66736` |
| **11%** | [Carbon](#carbon) | `7926` | `1450` | `10341` |
| **8%** | [Express](#express) | `6070` | `989` | `8250` |


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
    Reqs/sec      8725.73    5644.02   64554.88
    Latency        5.72ms     4.45ms   383.08ms
    HTTP codes:
      1xx - 0, 2xx - 92106, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7894
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7894
    Throughput:     1.83MB/s
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
    Reqs/sec      6243.94    1105.29    8378.94
    Latency        8.01ms     3.81ms   363.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24172.72    7861.48   35568.09
    Latency        2.07ms     2.05ms   184.85ms
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
    Reqs/sec     20531.48    6337.48   30631.00
    Latency        2.44ms     2.03ms   181.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     58572.98    3013.63   61513.91
    Latency        0.85ms    70.40us     2.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.32MB/s
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
    Reqs/sec     18709.38    9177.64   79845.01
    Latency        2.66ms     2.42ms   214.11ms
    HTTP codes:
      1xx - 0, 2xx - 91905, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8095
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8095
    Throughput:     3.89MB/s
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
    Reqs/sec     23758.27    6742.17   45977.67
    Latency        2.10ms     1.94ms   168.36ms
    HTTP codes:
      1xx - 0, 2xx - 98252, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1748
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1748
    Throughput:     5.34MB/s
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
    Reqs/sec     72342.69    1987.65   75543.58
    Latency      687.96us   198.23us    11.02ms
    HTTP codes:
      1xx - 0, 2xx - 96210, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3790
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3790
    Throughput:    11.01MB/s
  ```


