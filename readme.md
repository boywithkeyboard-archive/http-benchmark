## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69422` | `4245` | `88963` |
| **83%** | [Hyper Express](#hyper-express) | `57785` | `5788` | `97868` |
| **30%** | [Hono](#hono) | `21073` | `6548` | `29421` |
| **29%** | [Node (Default)](#node-default) | `19969` | `5834` | `77251` |
| **27%** | [Fastify](#fastify) | `19057` | `4112` | `35650` |
| **25%** | [Koa](#koa) | `17409` | `7870` | `61812` |
| **10%** | [Carbon](#carbon) | `7202` | `1213` | `10388` |
| **9%** | [Express](#express) | `6169` | `1107` | `8200` |


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
    Reqs/sec      8068.32    6390.69   72980.18
    Latency        6.18ms     4.64ms   396.46ms
    HTTP codes:
      1xx - 0, 2xx - 89348, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10652
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10652
    Throughput:     1.64MB/s
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
    Reqs/sec      6064.74    1092.00    8174.22
    Latency        8.24ms     3.99ms   382.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     19706.59    5319.68   35154.89
    Latency        2.53ms     2.15ms   188.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.47MB/s
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
    Reqs/sec     21218.21    6783.32   31200.90
    Latency        2.35ms     2.17ms   193.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     57480.75    3416.56   65326.64
    Latency        0.87ms    97.25us     4.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.17MB/s
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
    Reqs/sec     17887.72    8439.82   66397.60
    Latency        2.79ms     2.57ms   224.55ms
    HTTP codes:
      1xx - 0, 2xx - 92312, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7688
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7688
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
    Reqs/sec     19422.95    5348.40   60254.53
    Latency        2.57ms     2.03ms   179.21ms
    HTTP codes:
      1xx - 0, 2xx - 96398, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3602
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3602
    Throughput:     4.29MB/s
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
    Reqs/sec     68299.13    4697.13   82297.35
    Latency      728.64us   217.75us    11.25ms
    HTTP codes:
      1xx - 0, 2xx - 96010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3990
    Throughput:    10.38MB/s
  ```


