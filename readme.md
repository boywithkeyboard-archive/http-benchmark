## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69022` | `3860` | `79812` |
| **85%** | [Hyper Express](#hyper-express) | `58455` | `3683` | `66246` |
| **30%** | [Node (Default)](#node-default) | `20503` | `5797` | `68769` |
| **29%** | [Fastify](#fastify) | `20185` | `5324` | `36090` |
| **27%** | [Hono](#hono) | `18423` | `6137` | `30552` |
| **25%** | [Koa](#koa) | `17213` | `7905` | `65470` |
| **10%** | [Carbon](#carbon) | `7143` | `1076` | `10245` |
| **9%** | [Express](#express) | `6154` | `1099` | `8242` |


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
    Reqs/sec      7898.71    5520.08   79116.81
    Latency        6.32ms     4.63ms   394.51ms
    HTTP codes:
      1xx - 0, 2xx - 91989, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8011
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8011
    Throughput:     1.65MB/s
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
    Reqs/sec      6317.56    1174.04    8383.84
    Latency        7.91ms     3.87ms   371.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     19459.10    4340.58   34600.40
    Latency        2.57ms     2.09ms   189.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.42MB/s
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
    Reqs/sec     20001.35    5681.01   29802.12
    Latency        2.50ms     2.19ms   196.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.52MB/s
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
    Reqs/sec     58031.95    3653.31   66618.16
    Latency        0.86ms    94.43us     3.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.24MB/s
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
    Reqs/sec     17729.30    7515.61   63136.36
    Latency        2.81ms     2.50ms   217.69ms
    HTTP codes:
      1xx - 0, 2xx - 92870, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7130
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7130
    Throughput:     3.73MB/s
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
    Reqs/sec     19986.03    4663.94   57959.26
    Latency        2.50ms     2.11ms   182.01ms
    HTTP codes:
      1xx - 0, 2xx - 97400, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2600
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2600
    Throughput:     4.45MB/s
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
    Reqs/sec     68681.45    5111.53   82134.11
    Latency      725.47us   185.11us     7.18ms
    HTTP codes:
      1xx - 0, 2xx - 97510, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2490
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2490
    Throughput:    10.60MB/s
  ```


