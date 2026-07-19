## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73512` | `5401` | `88031` |
| **83%** | [Hyper Express](#hyper-express) | `60852` | `3719` | `68829` |
| **31%** | [Hono](#hono) | `22743` | `6708` | `30738` |
| **30%** | [Fastify](#fastify) | `22328` | `5776` | `37258` |
| **29%** | [Node (Default)](#node-default) | `21670` | `6062` | `68260` |
| **25%** | [Koa](#koa) | `18610` | `8162` | `65103` |
| **11%** | [Carbon](#carbon) | `7827` | `1389` | `10590` |
| **9%** | [Express](#express) | `6587` | `1134` | `8626` |


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
    Reqs/sec      8273.27    5029.58   65239.86
    Latency        6.03ms     4.68ms   410.57ms
    HTTP codes:
      1xx - 0, 2xx - 92838, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7162
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7162
    Throughput:     1.75MB/s
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
    Reqs/sec      6582.19    1136.23    8464.32
    Latency        7.59ms     3.72ms   357.33ms
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
    Reqs/sec     21693.70    5524.34   37199.99
    Latency        2.30ms     2.08ms   186.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.92MB/s
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
    Reqs/sec     21737.14    5627.63   31646.22
    Latency        2.30ms     2.27ms   199.12ms
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
    Reqs/sec     61431.59    3494.01   71639.73
    Latency      811.86us    91.19us     3.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.72MB/s
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
    Reqs/sec     18102.90    9217.75   75049.85
    Latency        2.76ms     2.30ms   203.48ms
    HTTP codes:
      1xx - 0, 2xx - 90693, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9307
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9307
    Throughput:     3.71MB/s
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
    Reqs/sec     22844.15    6610.85   65905.12
    Latency        2.19ms     2.02ms   174.21ms
    HTTP codes:
      1xx - 0, 2xx - 96729, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3271
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3271
    Throughput:     5.06MB/s
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
    Reqs/sec     72405.27    3878.15   85962.30
    Latency      688.15us   162.64us     7.86ms
    HTTP codes:
      1xx - 0, 2xx - 95885, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4115
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4115
    Throughput:    10.99MB/s
  ```


