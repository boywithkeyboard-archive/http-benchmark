## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `130177` | `5945` | `145530` |
| **95%** | [Hyper Express](#hyper-express) | `123642` | `5158` | `126104` |
| **58%** | [Node (Default)](#node-default) | `75052` | `18204` | `114691` |
| **56%** | [Hono](#hono) | `72791` | `20286` | `87307` |
| **54%** | [Fastify](#fastify) | `70585` | `17596` | `89051` |
| **52%** | [Koa](#koa) | `67937` | `22020` | `136977` |
| **21%** | [Carbon](#carbon) | `26689` | `7651` | `38773` |
| **14%** | [Express](#express) | `18051` | `3739` | `24508` |


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
    Reqs/sec     29793.56   15489.70  125309.44
    Latency        1.67ms     2.71ms   224.73ms
    HTTP codes:
      1xx - 0, 2xx - 90625, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9375
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9375
    Throughput:     6.14MB/s
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
    Reqs/sec     22015.89   13233.08  120224.98
    Latency        2.26ms     1.99ms   175.89ms
    HTTP codes:
      1xx - 0, 2xx - 89264, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10736
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10736
    Throughput:     5.64MB/s
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
    Reqs/sec     79305.86   21566.78  123047.07
    Latency      628.30us     0.89ms    71.71ms
    HTTP codes:
      1xx - 0, 2xx - 85003, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14997
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14971
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 26
    Throughput:    15.30MB/s
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
    Reqs/sec     70090.92   22244.19  133082.77
    Latency      712.48us     0.98ms    82.51ms
    HTTP codes:
      1xx - 0, 2xx - 90822, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9178
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9178
    Throughput:    14.37MB/s
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
    Reqs/sec    122232.68    6452.03  125691.06
    Latency      406.95us   165.84us     9.29ms
    HTTP codes:
      1xx - 0, 2xx - 93041, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6959
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6959
    Throughput:    16.16MB/s
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
    Reqs/sec     67310.30   19400.78  134164.69
    Latency      740.84us   740.58us    58.14ms
    HTTP codes:
      1xx - 0, 2xx - 91811, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8189
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8189
    Throughput:    13.97MB/s
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
    Reqs/sec     75207.92   19439.29  122340.88
    Latency      663.22us   599.81us    48.48ms
    HTTP codes:
      1xx - 0, 2xx - 96963, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3037
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3037
    Throughput:    16.69MB/s
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
    Reqs/sec    128476.83    4236.48  133857.18
    Latency      387.24us   127.34us     7.34ms
    HTTP codes:
      1xx - 0, 2xx - 96450, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3550
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3550
    Throughput:    19.61MB/s
  ```


