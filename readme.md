## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73784` | `5728` | `84187` |
| **85%** | [Hyper Express](#hyper-express) | `62854` | `3895` | `67658` |
| **36%** | [Node (Default)](#node-default) | `26493` | `7683` | `53993` |
| **32%** | [Fastify](#fastify) | `23759` | `6951` | `35602` |
| **28%** | [Hono](#hono) | `20591` | `5659` | `29537` |
| **24%** | [Koa](#koa) | `17851` | `6220` | `55595` |
| **11%** | [Carbon](#carbon) | `8355` | `1446` | `10464` |
| **9%** | [Express](#express) | `6573` | `1064` | `8547` |


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
    Reqs/sec      8576.40    3994.79   54722.94
    Latency        5.82ms     4.20ms   364.52ms
    HTTP codes:
      1xx - 0, 2xx - 94349, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5651
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5651
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
    Reqs/sec      6589.01    1070.38    8552.50
    Latency        7.58ms     3.57ms   340.40ms
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
    Reqs/sec     24257.70    7199.30   34994.14
    Latency        2.06ms     1.99ms   179.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.50MB/s
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
    Reqs/sec     21031.20    5713.84   29991.32
    Latency        2.38ms     1.95ms   174.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     63010.04    3663.04   65959.62
    Latency      790.82us    66.89us     3.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     17386.82    5718.01   47012.23
    Latency        2.87ms     2.37ms   205.20ms
    HTTP codes:
      1xx - 0, 2xx - 95444, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4556
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4556
    Throughput:     3.75MB/s
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
    Reqs/sec     27084.52    7894.07   45867.96
    Latency        1.84ms     1.81ms   161.31ms
    HTTP codes:
      1xx - 0, 2xx - 98271, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1729
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1729
    Throughput:     6.09MB/s
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
    Reqs/sec     74152.53    6841.48   93210.44
    Latency      671.23us   188.01us    10.25ms
    HTTP codes:
      1xx - 0, 2xx - 97908, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2092
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2092
    Throughput:    11.48MB/s
  ```


