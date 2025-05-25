## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73637` | `5032` | `80265` |
| **84%** | [Hyper Express](#hyper-express) | `61730` | `3368` | `70123` |
| **36%** | [Node (Default)](#node-default) | `26721` | `8360` | `59255` |
| **33%** | [Fastify](#fastify) | `24095` | `7227` | `35924` |
| **28%** | [Hono](#hono) | `20350` | `5495` | `29547` |
| **25%** | [Koa](#koa) | `18653` | `6491` | `49764` |
| **11%** | [Carbon](#carbon) | `8062` | `1367` | `10144` |
| **9%** | [Express](#express) | `6333` | `1009` | `8352` |


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
    Reqs/sec      8717.37    5018.25   56999.15
    Latency        5.72ms     4.27ms   369.27ms
    HTTP codes:
      1xx - 0, 2xx - 92040, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7960
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7960
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
    Reqs/sec      6412.19    1037.07    8344.97
    Latency        7.79ms     3.70ms   355.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     23903.26    7195.02   35845.65
    Latency        2.09ms     2.00ms   180.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     20671.29    5546.10   30253.09
    Latency        2.42ms     2.01ms   183.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     61039.78    3514.52   63306.56
    Latency      817.05us    75.24us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.67MB/s
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
    Reqs/sec     19065.04    6919.09   60781.56
    Latency        2.62ms     2.40ms   211.98ms
    HTTP codes:
      1xx - 0, 2xx - 92756, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7244
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7244
    Throughput:     4.00MB/s
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
    Reqs/sec     27009.76    8436.71   59659.53
    Latency        1.85ms     2.02ms   171.43ms
    HTTP codes:
      1xx - 0, 2xx - 96473, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3527
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3527
    Throughput:     5.96MB/s
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
    Reqs/sec     73068.66    4234.28   80612.36
    Latency      682.09us   167.15us     7.48ms
    HTTP codes:
      1xx - 0, 2xx - 97656, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2344
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2344
    Throughput:    11.29MB/s
  ```


