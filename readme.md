## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73584` | `4512` | `82349` |
| **83%** | [Hyper Express](#hyper-express) | `61285` | `4041` | `70124` |
| **33%** | [Hono](#hono) | `24071` | `7067` | `32007` |
| **31%** | [Node (Default)](#node-default) | `22608` | `7481` | `78001` |
| **29%** | [Fastify](#fastify) | `21056` | `4748` | `36484` |
| **26%** | [Koa](#koa) | `19183` | `8030` | `64846` |
| **10%** | [Carbon](#carbon) | `7716` | `1293` | `10654` |
| **9%** | [Express](#express) | `6584` | `1127` | `8520` |


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
    Reqs/sec      8377.35    5562.47   67534.50
    Latency        5.96ms     4.45ms   378.93ms
    HTTP codes:
      1xx - 0, 2xx - 92044, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7956
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7956
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
    Reqs/sec      6518.12    1149.65    8493.49
    Latency        7.67ms     3.77ms   359.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     22943.82    6822.66   36138.31
    Latency        2.18ms     2.10ms   190.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.21MB/s
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
    Reqs/sec     22994.80    6696.72   31096.60
    Latency        2.17ms     2.12ms   188.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.20MB/s
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
    Reqs/sec     61578.34    4021.38   67310.91
    Latency      809.30us    90.43us     2.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     18608.78    7883.26   62744.01
    Latency        2.68ms     2.32ms   205.24ms
    HTTP codes:
      1xx - 0, 2xx - 93299, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6701
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6701
    Throughput:     3.93MB/s
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
    Reqs/sec     21588.88    5456.07   54643.01
    Latency        2.31ms     1.87ms   163.47ms
    HTTP codes:
      1xx - 0, 2xx - 97627, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2373
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2373
    Throughput:     4.83MB/s
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
    Reqs/sec     72457.96    4380.35   87078.33
    Latency      688.05us   167.01us     9.58ms
    HTTP codes:
      1xx - 0, 2xx - 96912, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3088
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3088
    Throughput:    11.11MB/s
  ```


