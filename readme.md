## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73295` | `3314` | `81307` |
| **83%** | [Hyper Express](#hyper-express) | `60766` | `3974` | `63416` |
| **35%** | [Node (Default)](#node-default) | `25325` | `7866` | `58245` |
| **31%** | [Fastify](#fastify) | `22679` | `6408` | `34545` |
| **28%** | [Hono](#hono) | `20232` | `5964` | `29542` |
| **26%** | [Koa](#koa) | `18803` | `8818` | `73444` |
| **11%** | [Carbon](#carbon) | `8137` | `1484` | `10235` |
| **8%** | [Express](#express) | `6090` | `1013` | `8174` |


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
    Reqs/sec      8916.38    7039.58   80987.17
    Latency        5.59ms     4.41ms   380.86ms
    HTTP codes:
      1xx - 0, 2xx - 89116, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10884
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10884
    Throughput:     1.81MB/s
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
    Reqs/sec      6956.90    6392.09   79686.68
    Latency        7.18ms     4.04ms   372.00ms
    HTTP codes:
      1xx - 0, 2xx - 88669, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11331
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11331
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
    Reqs/sec     23100.73    7330.11   35193.47
    Latency        2.16ms     2.07ms   188.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.24MB/s
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
    Reqs/sec     20443.30    6158.51   29945.35
    Latency        2.45ms     2.07ms   184.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     61003.67    3800.76   65836.36
    Latency      817.48us    79.86us     3.26ms
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
    Reqs/sec     18661.54    7977.75   71179.07
    Latency        2.67ms     2.51ms   219.55ms
    HTTP codes:
      1xx - 0, 2xx - 92871, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7129
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7129
    Throughput:     3.92MB/s
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
    Reqs/sec     25435.56    9729.55   80855.39
    Latency        1.96ms     2.00ms   172.11ms
    HTTP codes:
      1xx - 0, 2xx - 94843, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5157
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5157
    Throughput:     5.52MB/s
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
    Reqs/sec     73011.21    3120.66   85263.04
    Latency      682.46us   141.46us     5.99ms
    HTTP codes:
      1xx - 0, 2xx - 97405, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2595
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2595
    Throughput:    11.26MB/s
  ```


