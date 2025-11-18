## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74671` | `2607` | `83723` |
| **83%** | [Hyper Express](#hyper-express) | `61838` | `3427` | `64748` |
| **35%** | [Node (Default)](#node-default) | `26308` | `7921` | `63404` |
| **32%** | [Fastify](#fastify) | `23835` | `6971` | `36138` |
| **28%** | [Hono](#hono) | `21016` | `5771` | `29582` |
| **26%** | [Koa](#koa) | `19133` | `8030` | `62096` |
| **11%** | [Carbon](#carbon) | `8118` | `1421` | `10396` |
| **9%** | [Express](#express) | `6536` | `1094` | `8427` |


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
    Reqs/sec      8971.88    6436.52   75462.31
    Latency        5.56ms     4.42ms   378.71ms
    HTTP codes:
      1xx - 0, 2xx - 90981, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9019
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9019
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
    Reqs/sec      7042.27    5778.88   75993.90
    Latency        7.08ms     4.03ms   365.92ms
    HTTP codes:
      1xx - 0, 2xx - 90370, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9630
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9630
    Throughput:     1.82MB/s
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
    Reqs/sec     24100.21    6915.96   36358.78
    Latency        2.07ms     1.99ms   175.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     21112.15    6061.35   29735.56
    Latency        2.37ms     2.10ms   184.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     61739.28    3384.36   69678.39
    Latency      808.91us    72.02us     2.60ms
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
    Reqs/sec     19126.55   10154.55   81341.41
    Latency        2.60ms     2.44ms   212.33ms
    HTTP codes:
      1xx - 0, 2xx - 88779, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11221
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11221
    Throughput:     3.85MB/s
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
    Reqs/sec     27126.98    8882.73   75210.77
    Latency        1.84ms     2.04ms   176.00ms
    HTTP codes:
      1xx - 0, 2xx - 96458, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3542
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3542
    Throughput:     6.00MB/s
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
    Reqs/sec     74912.50    2862.79   82250.80
    Latency      665.11us   155.77us     8.48ms
    HTTP codes:
      1xx - 0, 2xx - 96969, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3031
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3031
    Throughput:    11.49MB/s
  ```


