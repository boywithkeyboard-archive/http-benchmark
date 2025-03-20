## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74406` | `5615` | `86996` |
| **84%** | [Hyper Express](#hyper-express) | `62698` | `3688` | `68115` |
| **35%** | [Node (Default)](#node-default) | `26412` | `7822` | `59415` |
| **35%** | [Fastify](#fastify) | `26069` | `8317` | `36424` |
| **27%** | [Hono](#hono) | `20453` | `5578` | `30473` |
| **26%** | [Koa](#koa) | `19093` | `6720` | `59089` |
| **11%** | [Carbon](#carbon) | `8313` | `1400` | `10378` |
| **9%** | [Express](#express) | `6429` | `1062` | `8484` |


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
    Reqs/sec      8845.66    4019.79   51832.36
    Latency        5.63ms     4.33ms   374.01ms
    HTTP codes:
      1xx - 0, 2xx - 93655, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6345
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6345
    Throughput:     1.88MB/s
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
    Reqs/sec      6912.64    4490.76   56080.13
    Latency        7.22ms     3.93ms   361.11ms
    HTTP codes:
      1xx - 0, 2xx - 91733, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8267
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8267
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
    Reqs/sec     26066.95    8313.04   36898.68
    Latency        1.92ms     2.06ms   184.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.90MB/s
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
    Reqs/sec     20834.36    5700.02   29648.30
    Latency        2.40ms     1.96ms   177.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     63535.73    4001.13   68035.14
    Latency      784.65us    71.48us     3.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.02MB/s
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
    Reqs/sec     19271.67    6968.06   60028.88
    Latency        2.59ms     2.35ms   206.90ms
    HTTP codes:
      1xx - 0, 2xx - 94007, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5993
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5993
    Throughput:     4.09MB/s
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
    Reqs/sec     27015.54    8058.71   52067.15
    Latency        1.85ms     1.89ms   160.64ms
    HTTP codes:
      1xx - 0, 2xx - 97210, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2790
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2790
    Throughput:     6.01MB/s
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
    Reqs/sec     74383.96    4907.16   79293.16
    Latency      669.79us   161.44us     9.01ms
    HTTP codes:
      1xx - 0, 2xx - 97385, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2615
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2615
    Throughput:    11.46MB/s
  ```


