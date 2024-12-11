## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75452` | `7059` | `97146` |
| **84%** | [Hyper Express](#hyper-express) | `63070` | `4053` | `72429` |
| **35%** | [Node (Default)](#node-default) | `26079` | `8018` | `54141` |
| **31%** | [Fastify](#fastify) | `23667` | `6983` | `36786` |
| **28%** | [Hono](#hono) | `20823` | `5642` | `29688` |
| **24%** | [Koa](#koa) | `17830` | `6195` | `56550` |
| **11%** | [Carbon](#carbon) | `8268` | `1421` | `10795` |
| **9%** | [Express](#express) | `6466` | `1034` | `8685` |


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
    Reqs/sec      8550.53    2990.47   46544.98
    Latency        5.85ms     4.17ms   365.23ms
    HTTP codes:
      1xx - 0, 2xx - 96436, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3564
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3564
    Throughput:     1.87MB/s
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
    Reqs/sec      6453.81    1014.23    8438.35
    Latency        7.75ms     3.51ms   339.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     23383.79    7094.82   35062.49
    Latency        2.14ms     2.02ms   183.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.30MB/s
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
    Reqs/sec     21099.46    6015.45   29825.70
    Latency        2.37ms     1.96ms   174.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     62095.95    3819.21   65272.90
    Latency      802.83us    68.27us     2.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     18772.71    6148.67   45966.70
    Latency        2.66ms     2.33ms   202.01ms
    HTTP codes:
      1xx - 0, 2xx - 94898, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5102
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5102
    Throughput:     4.03MB/s
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
    Reqs/sec     24928.44    7119.13   47290.13
    Latency        2.00ms     1.89ms   163.02ms
    HTTP codes:
      1xx - 0, 2xx - 97968, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2032
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2032
    Throughput:     5.59MB/s
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
    Reqs/sec     74330.96    6857.36   92446.88
    Latency      670.09us   181.63us     9.37ms
    HTTP codes:
      1xx - 0, 2xx - 98328, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1672
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1672
    Throughput:    11.57MB/s
  ```


