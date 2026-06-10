## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72917` | `3705` | `82368` |
| **84%** | [Hyper Express](#hyper-express) | `61168` | `5284` | `69201` |
| **33%** | [Node (Default)](#node-default) | `23762` | `6935` | `71427` |
| **31%** | [Fastify](#fastify) | `22655` | `6719` | `34722` |
| **31%** | [Hono](#hono) | `22436` | `6482` | `30481` |
| **27%** | [Koa](#koa) | `19565` | `9253` | `78430` |
| **11%** | [Carbon](#carbon) | `7825` | `1372` | `10669` |
| **9%** | [Express](#express) | `6482` | `1119` | `8421` |


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
    Reqs/sec      8629.77    7535.42   78666.06
    Latency        5.78ms     4.34ms   372.35ms
    HTTP codes:
      1xx - 0, 2xx - 87908, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12092
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12092
    Throughput:     1.72MB/s
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
    Reqs/sec      6314.46    1014.46    8368.54
    Latency        7.91ms     3.81ms   365.70ms
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
    Reqs/sec     23112.79    6615.85   35731.42
    Latency        2.16ms     1.99ms   179.62ms
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
    Reqs/sec     22132.15    6255.81   30299.59
    Latency        2.26ms     2.05ms   184.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.00MB/s
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
    Reqs/sec     62195.78    3603.81   70290.14
    Latency      801.45us    83.69us     2.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18124.94    8711.30   72992.06
    Latency        2.75ms     2.42ms   208.60ms
    HTTP codes:
      1xx - 0, 2xx - 91967, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8033
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8033
    Throughput:     3.77MB/s
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
    Reqs/sec     23737.54    6789.87   67120.69
    Latency        2.10ms     1.89ms   164.62ms
    HTTP codes:
      1xx - 0, 2xx - 96446, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3554
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3554
    Throughput:     5.24MB/s
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
    Reqs/sec     74039.03    4705.21   86620.96
    Latency      671.06us   233.26us    11.41ms
    HTTP codes:
      1xx - 0, 2xx - 96283, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3717
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3717
    Throughput:    11.28MB/s
  ```


