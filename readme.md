## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74398` | `4433` | `81445` |
| **84%** | [Hyper Express](#hyper-express) | `62434` | `2638` | `66611` |
| **37%** | [Node (Default)](#node-default) | `27220` | `8643` | `59501` |
| **33%** | [Fastify](#fastify) | `24353` | `7642` | `36993` |
| **29%** | [Hono](#hono) | `21348` | `6261` | `30588` |
| **25%** | [Koa](#koa) | `18733` | `7008` | `58357` |
| **11%** | [Carbon](#carbon) | `8149` | `1357` | `10213` |
| **9%** | [Express](#express) | `6484` | `1031` | `8431` |


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
    Reqs/sec      8677.84    4440.37   57110.43
    Latency        5.75ms     4.24ms   368.20ms
    HTTP codes:
      1xx - 0, 2xx - 93524, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6476
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6476
    Throughput:     1.84MB/s
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
    Reqs/sec      6456.01    1051.82    8445.94
    Latency        7.74ms     3.65ms   348.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     25494.81    7739.46   36419.08
    Latency        1.96ms     2.06ms   184.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.78MB/s
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
    Reqs/sec     20797.62    5925.87   29857.69
    Latency        2.40ms     2.03ms   180.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     61414.78    3551.53   64248.20
    Latency      812.13us    68.91us     3.07ms
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
    Reqs/sec     19868.20    7728.53   60199.45
    Latency        2.51ms     2.47ms   215.38ms
    HTTP codes:
      1xx - 0, 2xx - 92865, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7135
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7135
    Throughput:     4.17MB/s
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
    Reqs/sec     26676.92    8140.23   48139.33
    Latency        1.87ms     1.99ms   172.73ms
    HTTP codes:
      1xx - 0, 2xx - 97975, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2025
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2025
    Throughput:     5.99MB/s
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
    Reqs/sec     73837.17    4268.19   80746.23
    Latency      674.99us   153.55us     6.48ms
    HTTP codes:
      1xx - 0, 2xx - 97386, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2614
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2614
    Throughput:    11.37MB/s
  ```


