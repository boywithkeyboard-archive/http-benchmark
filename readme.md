## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72723` | `6660` | `97085` |
| **85%** | [Hyper Express](#hyper-express) | `61707` | `4585` | `68809` |
| **37%** | [Node (Default)](#node-default) | `26849` | `8560` | `48808` |
| **35%** | [Fastify](#fastify) | `25749` | `7925` | `36214` |
| **28%** | [Hono](#hono) | `20492` | `6293` | `30086` |
| **25%** | [Koa](#koa) | `17822` | `6786` | `51847` |
| **11%** | [Carbon](#carbon) | `7837` | `1343` | `10086` |
| **8%** | [Express](#express) | `6121` | `924` | `9041` |


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
    Reqs/sec      8607.44    5379.82   65073.91
    Latency        5.80ms     4.50ms   380.95ms
    HTTP codes:
      1xx - 0, 2xx - 91259, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8741
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8741
    Throughput:     1.78MB/s
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
    Reqs/sec      6334.55    1048.38    8774.13
    Latency        7.89ms     3.80ms   361.97ms
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
    Reqs/sec     24791.17    8052.46   36659.31
    Latency        2.02ms     1.79ms   171.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.61MB/s
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
    Reqs/sec     20159.25    5816.18   29692.19
    Latency        2.48ms     2.00ms   183.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.55MB/s
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
    Reqs/sec     62645.42    3989.57   66462.99
    Latency      796.05us   102.63us     7.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     17996.32    6897.20   57749.66
    Latency        2.77ms     2.40ms   209.32ms
    HTTP codes:
      1xx - 0, 2xx - 93487, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6513
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6513
    Throughput:     3.81MB/s
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
    Reqs/sec     25938.58    8063.20   60654.89
    Latency        1.92ms     1.90ms   163.70ms
    HTTP codes:
      1xx - 0, 2xx - 96310, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3690
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3690
    Throughput:     5.72MB/s
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
    Reqs/sec     71958.87    5193.53   79425.76
    Latency      692.82us   186.41us     9.81ms
    HTTP codes:
      1xx - 0, 2xx - 96861, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3139
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3139
    Throughput:    11.02MB/s
  ```


