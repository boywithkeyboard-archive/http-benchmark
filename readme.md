## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `78458` | `3010` | `84114` |
| **88%** | [Hyper Express](#hyper-express) | `68857` | `2835` | `71725` |
| **45%** | [Node (Default)](#node-default) | `35418` | `10100` | `80345` |
| **44%** | [Fastify](#fastify) | `34731` | `10626` | `52296` |
| **40%** | [Koa](#koa) | `31391` | `13492` | `74364` |
| **37%** | [Hono](#hono) | `28917` | `7994` | `47461` |
| **12%** | [Carbon](#carbon) | `9272` | `2223` | `13050` |
| **10%** | [Express](#express) | `7510` | `1755` | `10513` |


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
    Reqs/sec      9962.43    6321.54   77754.76
    Latency        5.01ms     4.42ms   376.52ms
    HTTP codes:
      1xx - 0, 2xx - 92750, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7250
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7250
    Throughput:     2.10MB/s
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
    Reqs/sec      7874.47    6721.86   81950.99
    Latency        6.34ms     3.82ms   354.11ms
    HTTP codes:
      1xx - 0, 2xx - 89852, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10148
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10148
    Throughput:     2.03MB/s
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
    Reqs/sec     32737.23    8596.84   50399.39
    Latency        1.53ms     1.93ms   168.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.43MB/s
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
    Reqs/sec     28758.19    7821.90   46626.68
    Latency        1.74ms     2.08ms   181.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.50MB/s
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
    Reqs/sec     69137.47    3137.98   72161.63
    Latency      721.54us    76.97us     4.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.82MB/s
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
    Reqs/sec     29293.83   11929.96   75557.73
    Latency        1.70ms     2.21ms   193.44ms
    HTTP codes:
      1xx - 0, 2xx - 92814, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7186
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7186
    Throughput:     6.14MB/s
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
    Reqs/sec     35188.82    8913.03   72306.50
    Latency        1.42ms     1.78ms   150.12ms
    HTTP codes:
      1xx - 0, 2xx - 96816, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3184
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3184
    Throughput:     7.80MB/s
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
    Reqs/sec     79630.70    3313.73   89591.26
    Latency      625.60us   140.11us     7.08ms
    HTTP codes:
      1xx - 0, 2xx - 96747, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3253
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3253
    Throughput:    12.19MB/s
  ```


