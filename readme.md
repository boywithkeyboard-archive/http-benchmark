## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73704` | `5956` | `90258` |
| **83%** | [Hyper Express](#hyper-express) | `61383` | `4106` | `72275` |
| **29%** | [Fastify](#fastify) | `21678` | `6662` | `36247` |
| **29%** | [Hono](#hono) | `21196` | `6403` | `31015` |
| **27%** | [Node (Default)](#node-default) | `19962` | `6190` | `73834` |
| **25%** | [Koa](#koa) | `18193` | `8203` | `67339` |
| **10%** | [Carbon](#carbon) | `7377` | `1286` | `10509` |
| **8%** | [Express](#express) | `6098` | `1048` | `8411` |


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
    Reqs/sec      8615.76    7231.97   73639.37
    Latency        5.79ms     4.37ms   373.90ms
    HTTP codes:
      1xx - 0, 2xx - 88270, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11730
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11730
    Throughput:     1.73MB/s
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
    Reqs/sec      6330.31    1164.95    8351.00
    Latency        7.89ms     3.75ms   363.22ms
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
    Reqs/sec     21152.73    5945.10   36419.65
    Latency        2.36ms     2.13ms   191.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     21144.40    6118.03   30812.25
    Latency        2.36ms     2.22ms   199.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     59766.54    3190.64   68225.14
    Latency      834.24us    87.71us     3.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.49MB/s
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
    Reqs/sec     21078.64   12223.78   79852.26
    Latency        2.37ms     2.57ms   220.77ms
    HTTP codes:
      1xx - 0, 2xx - 85569, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14431
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14431
    Throughput:     4.08MB/s
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
    Reqs/sec     19797.16    4779.95   64530.92
    Latency        2.52ms     1.90ms   163.78ms
    HTTP codes:
      1xx - 0, 2xx - 96933, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3067
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3067
    Throughput:     4.39MB/s
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
    Reqs/sec     73505.17    4187.27   83033.86
    Latency      676.93us   235.83us    13.39ms
    HTTP codes:
      1xx - 0, 2xx - 95287, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4713
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4713
    Throughput:    11.09MB/s
  ```


