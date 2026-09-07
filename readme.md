## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71522` | `3584` | `80575` |
| **85%** | [Hyper Express](#hyper-express) | `60439` | `3485` | `66952` |
| **31%** | [Node (Default)](#node-default) | `22392` | `6427` | `69891` |
| **31%** | [Hono](#hono) | `21955` | `6678` | `29656` |
| **29%** | [Fastify](#fastify) | `20822` | `5407` | `36158` |
| **27%** | [Koa](#koa) | `19657` | `8431` | `67455` |
| **11%** | [Carbon](#carbon) | `7670` | `1257` | `10396` |
| **9%** | [Express](#express) | `6303` | `1073` | `8314` |


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
    Reqs/sec      8121.31    6377.35   71599.70
    Latency        6.14ms     4.56ms   387.24ms
    HTTP codes:
      1xx - 0, 2xx - 89506, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10494
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10494
    Throughput:     1.65MB/s
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
    Reqs/sec      6347.99    1059.36    8239.12
    Latency        7.87ms     3.80ms   362.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     22411.84    6514.85   36180.48
    Latency        2.23ms     2.21ms   197.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.09MB/s
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
    Reqs/sec     22722.75    6346.57   30097.27
    Latency        2.20ms     2.24ms   199.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.13MB/s
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
    Reqs/sec     61094.16    4023.15   78835.59
    Latency      818.72us    97.09us     3.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.65MB/s
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
    Reqs/sec     21146.32   10828.67   78176.62
    Latency        2.36ms     2.38ms   210.03ms
    HTTP codes:
      1xx - 0, 2xx - 88962, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11038
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11038
    Throughput:     4.25MB/s
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
    Reqs/sec     22133.50    6204.02   69329.69
    Latency        2.25ms     2.02ms   178.22ms
    HTTP codes:
      1xx - 0, 2xx - 96698, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3302
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3302
    Throughput:     4.90MB/s
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
    Reqs/sec     73190.92    4283.12   82203.09
    Latency      679.59us   197.06us    13.29ms
    HTTP codes:
      1xx - 0, 2xx - 96643, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3357
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3357
    Throughput:    11.20MB/s
  ```


