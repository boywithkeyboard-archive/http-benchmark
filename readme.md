## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `130833` | `4226` | `137444` |
| **89%** | [Hyper Express](#hyper-express) | `116714` | `4357` | `121850` |
| **59%** | [Node (Default)](#node-default) | `77717` | `17386` | `126046` |
| **57%** | [Fastify](#fastify) | `75146` | `18839` | `91583` |
| **54%** | [Hono](#hono) | `70678` | `18858` | `86875` |
| **51%** | [Koa](#koa) | `67245` | `19347` | `106667` |
| **21%** | [Carbon](#carbon) | `27534` | `7921` | `39471` |
| **14%** | [Express](#express) | `18746` | `3768` | `26441` |


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
    Reqs/sec     30887.71   16911.06  118833.94
    Latency        1.62ms     2.86ms   238.67ms
    HTTP codes:
      1xx - 0, 2xx - 88858, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11142
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11142
    Throughput:     6.23MB/s
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
    Reqs/sec     20483.12   11974.96  121964.08
    Latency        2.43ms     1.98ms   181.40ms
    HTTP codes:
      1xx - 0, 2xx - 91031, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8969
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8969
    Throughput:     5.36MB/s
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
    Reqs/sec     77460.61   19988.43  122099.36
    Latency      643.11us     0.95ms    73.64ms
    HTTP codes:
      1xx - 0, 2xx - 86955, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13045
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13045
    Throughput:    15.28MB/s
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
    Reqs/sec     71605.31   19387.53  124366.74
    Latency      696.96us     0.87ms    69.59ms
    HTTP codes:
      1xx - 0, 2xx - 93386, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6614
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6614
    Throughput:    15.11MB/s
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
    Reqs/sec    119696.79    5514.63  124335.41
    Latency      415.58us   167.22us     9.22ms
    HTTP codes:
      1xx - 0, 2xx - 94442, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5558
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5558
    Throughput:    16.06MB/s
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
    Reqs/sec     66991.83   19723.88  133651.37
    Latency      745.05us     1.10ms    82.30ms
    HTTP codes:
      1xx - 0, 2xx - 92545, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7455
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7455
    Throughput:    14.00MB/s
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
    Reqs/sec     77981.33   19499.50  145543.01
    Latency      640.21us   805.39us    66.71ms
    HTTP codes:
      1xx - 0, 2xx - 95072, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4928
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4928
    Throughput:    16.94MB/s
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
    Reqs/sec    129218.73    3275.20  133411.89
    Latency      385.30us   113.29us     5.40ms
    HTTP codes:
      1xx - 0, 2xx - 94149, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5851
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5851
    Throughput:    19.27MB/s
  ```


