## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74762` | `2311` | `81322` |
| **86%** | [Hyper Express](#hyper-express) | `64192` | `4534` | `76830` |
| **35%** | [Node (Default)](#node-default) | `25827` | `8361` | `71609` |
| **32%** | [Fastify](#fastify) | `23556` | `7334` | `35774` |
| **28%** | [Hono](#hono) | `21022` | `5534` | `29986` |
| **27%** | [Koa](#koa) | `19955` | `7393` | `60654` |
| **11%** | [Carbon](#carbon) | `8151` | `1384` | `10424` |
| **9%** | [Express](#express) | `6369` | `1055` | `8378` |


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
    Reqs/sec      8689.25    4725.36   65240.48
    Latency        5.74ms     4.31ms   371.80ms
    HTTP codes:
      1xx - 0, 2xx - 93945, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6055
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6055
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
    Reqs/sec      6886.25    5956.78   76384.79
    Latency        7.25ms     3.93ms   361.67ms
    HTTP codes:
      1xx - 0, 2xx - 89631, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10369
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10369
    Throughput:     1.77MB/s
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
    Reqs/sec     23720.11    7481.46   35396.10
    Latency        2.11ms     2.08ms   186.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     20417.67    5671.49   29481.80
    Latency        2.45ms     1.96ms   179.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     63772.63    3395.39   67214.60
    Latency      781.38us    74.14us     3.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.06MB/s
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
    Reqs/sec     18380.36    8724.96   74588.36
    Latency        2.71ms     2.39ms   209.64ms
    HTTP codes:
      1xx - 0, 2xx - 92091, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7909
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7909
    Throughput:     3.83MB/s
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
    Reqs/sec     25683.46    8623.21   72774.91
    Latency        1.94ms     1.97ms   176.77ms
    HTTP codes:
      1xx - 0, 2xx - 96157, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3843
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3843
    Throughput:     5.65MB/s
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
    Reqs/sec     75540.90    3236.19   80686.15
    Latency      657.07us   178.29us    12.54ms
    HTTP codes:
      1xx - 0, 2xx - 96440, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3560
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3560
    Throughput:    11.55MB/s
  ```


