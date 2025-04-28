## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72238` | `4365` | `78205` |
| **86%** | [Hyper Express](#hyper-express) | `61833` | `4012` | `75329` |
| **34%** | [Node (Default)](#node-default) | `24688` | `7529` | `55460` |
| **31%** | [Fastify](#fastify) | `22384` | `6380` | `36228` |
| **29%** | [Hono](#hono) | `20799` | `5603` | `29430` |
| **26%** | [Koa](#koa) | `19060` | `6472` | `53212` |
| **11%** | [Carbon](#carbon) | `8115` | `1358` | `10296` |
| **9%** | [Express](#express) | `6492` | `1063` | `8358` |


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
    Reqs/sec      8822.55    5224.55   60898.56
    Latency        5.66ms     4.27ms   371.48ms
    HTTP codes:
      1xx - 0, 2xx - 90931, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9069
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9069
    Throughput:     1.82MB/s
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
    Reqs/sec      6437.47    1051.05    8241.50
    Latency        7.76ms     3.67ms   352.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     24424.71    7430.29   35888.68
    Latency        2.05ms     2.05ms   184.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.54MB/s
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
    Reqs/sec     20583.81    5431.55   28348.87
    Latency        2.43ms     1.96ms   178.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     60668.51    3368.30   62759.96
    Latency      821.37us    70.29us     3.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.62MB/s
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
    Reqs/sec     19020.09    7873.53   58802.17
    Latency        2.62ms     2.39ms   209.25ms
    HTTP codes:
      1xx - 0, 2xx - 92112, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7888
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7888
    Throughput:     3.96MB/s
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
    Reqs/sec     25428.94    7770.88   55185.39
    Latency        1.96ms     1.89ms   159.05ms
    HTTP codes:
      1xx - 0, 2xx - 96822, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3178
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3178
    Throughput:     5.63MB/s
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
    Reqs/sec     72190.00    4737.07   79852.47
    Latency      690.24us   158.81us     6.71ms
    HTTP codes:
      1xx - 0, 2xx - 97565, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2435
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2435
    Throughput:    11.14MB/s
  ```


