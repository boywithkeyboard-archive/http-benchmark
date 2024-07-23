## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74005` | `6659` | `84458` |
| **86%** | [Hyper Express](#hyper-express) | `63613` | `4518` | `76692` |
| **34%** | [Node (Default)](#node-default) | `25446` | `7292` | `40423` |
| **32%** | [Fastify](#fastify) | `23588` | `6931` | `33092` |
| **28%** | [Hono](#hono) | `20853` | `6138` | `29523` |
| **23%** | [Koa](#koa) | `17280` | `5878` | `50680` |
| **11%** | [Carbon](#carbon) | `7880` | `1312` | `10071` |
| **8%** | [Express](#express) | `6184` | `957` | `8311` |


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
    Reqs/sec      8268.65    3449.23   46777.38
    Latency        6.04ms     4.27ms   367.38ms
    HTTP codes:
      1xx - 0, 2xx - 95194, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4806
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4806
    Throughput:     1.79MB/s
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
    Reqs/sec      6277.74    1000.61    8293.89
    Latency        7.96ms     3.70ms   353.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     23081.91    6821.92   33723.08
    Latency        2.16ms     1.99ms   176.69ms
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
    Reqs/sec     20209.67    5176.41   28512.64
    Latency        2.47ms     2.00ms   180.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.57MB/s
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
    Reqs/sec     64939.43    3348.78   75035.49
    Latency      769.08us    66.79us     3.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.21MB/s
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
    Reqs/sec     17441.91    5622.07   47189.83
    Latency        2.86ms     2.40ms   213.20ms
    HTTP codes:
      1xx - 0, 2xx - 95198, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4802
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4802
    Throughput:     3.75MB/s
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
    Reqs/sec     24882.92    7250.99   46182.56
    Latency        2.01ms     1.96ms   169.76ms
    HTTP codes:
      1xx - 0, 2xx - 98123, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1877
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1877
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
    Reqs/sec     73524.51    6764.99   84979.19
    Latency      677.74us   179.57us     8.92ms
    HTTP codes:
      1xx - 0, 2xx - 97465, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2535
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2535
    Throughput:    11.34MB/s
  ```


