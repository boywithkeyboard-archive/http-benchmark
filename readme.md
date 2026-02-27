## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70067` | `3858` | `80966` |
| **83%** | [Hyper Express](#hyper-express) | `57945` | `3648` | `64450` |
| **31%** | [Node (Default)](#node-default) | `21800` | `5776` | `56652` |
| **29%** | [Hono](#hono) | `20464` | `6362` | `30931` |
| **29%** | [Fastify](#fastify) | `20185` | `5267` | `36012` |
| **25%** | [Koa](#koa) | `17546` | `8186` | `72632` |
| **11%** | [Carbon](#carbon) | `7788` | `1450` | `10467` |
| **9%** | [Express](#express) | `6255` | `1138` | `8342` |


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
    Reqs/sec      8535.55    5280.21   64403.21
    Latency        5.85ms     4.38ms   376.95ms
    HTTP codes:
      1xx - 0, 2xx - 92582, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7418
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7418
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
    Reqs/sec      6258.56    1148.40    8450.30
    Latency        7.98ms     3.81ms   366.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     21290.23    6081.43   36586.55
    Latency        2.35ms     2.16ms   190.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     22032.96    7157.12   31282.06
    Latency        2.27ms     2.20ms   195.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.98MB/s
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
    Reqs/sec     58181.19    3161.10   66817.97
    Latency        0.86ms    85.03us     3.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.27MB/s
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
    Reqs/sec     17054.66    8348.72   67450.79
    Latency        2.92ms     2.44ms   214.52ms
    HTTP codes:
      1xx - 0, 2xx - 91604, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8396
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8396
    Throughput:     3.54MB/s
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
    Reqs/sec     21903.63    6821.95   72613.27
    Latency        2.28ms     2.00ms   174.00ms
    HTTP codes:
      1xx - 0, 2xx - 95985, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4015
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4015
    Throughput:     4.82MB/s
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
    Reqs/sec     70232.35    3060.27   75642.99
    Latency      708.36us   181.37us    10.43ms
    HTTP codes:
      1xx - 0, 2xx - 96516, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3484
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3484
    Throughput:    10.73MB/s
  ```


