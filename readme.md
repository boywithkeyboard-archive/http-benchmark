## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74613` | `2996` | `78655` |
| **84%** | [Hyper Express](#hyper-express) | `62916` | `3997` | `67756` |
| **32%** | [Fastify](#fastify) | `23648` | `7388` | `35761` |
| **31%** | [Node (Default)](#node-default) | `23482` | `7424` | `67000` |
| **28%** | [Hono](#hono) | `21012` | `6018` | `29473` |
| **25%** | [Koa](#koa) | `18701` | `8057` | `66566` |
| **11%** | [Carbon](#carbon) | `8266` | `1502` | `10312` |
| **8%** | [Express](#express) | `6277` | `980` | `8336` |


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
    Reqs/sec      9655.01    8481.24   81125.64
    Latency        5.16ms     4.39ms   380.59ms
    HTTP codes:
      1xx - 0, 2xx - 85889, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14111
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14111
    Throughput:     1.89MB/s
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
    Reqs/sec      6897.97    5924.40   71725.70
    Latency        7.24ms     4.06ms   371.99ms
    HTTP codes:
      1xx - 0, 2xx - 89753, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10247
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10247
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
    Reqs/sec     23506.46    7022.75   35099.52
    Latency        2.13ms     2.00ms   177.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.33MB/s
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
    Reqs/sec     21352.53    6487.42   29711.43
    Latency        2.34ms     2.03ms   182.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     62676.63    3100.58   66173.57
    Latency      795.25us    73.42us     3.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     18718.76    7728.34   65464.87
    Latency        2.67ms     2.46ms   216.81ms
    HTTP codes:
      1xx - 0, 2xx - 93211, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6789
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6789
    Throughput:     3.94MB/s
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
    Reqs/sec     26737.06    8666.75   71570.69
    Latency        1.87ms     1.90ms   164.00ms
    HTTP codes:
      1xx - 0, 2xx - 96191, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3809
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3809
    Throughput:     5.89MB/s
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
    Reqs/sec     74739.58    3253.01   82008.65
    Latency      666.41us   140.82us     5.74ms
    HTTP codes:
      1xx - 0, 2xx - 97235, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2765
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2765
    Throughput:    11.50MB/s
  ```


