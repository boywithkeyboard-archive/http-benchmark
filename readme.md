## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `116727` | `12582` | `140059` |
| **96%** | [Hyper Express](#hyper-express) | `111577` | `6500` | `115032` |
| **62%** | [Node (Default)](#node-default) | `72092` | `16905` | `109807` |
| **60%** | [Fastify](#fastify) | `70478` | `16736` | `87523` |
| **54%** | [Koa](#koa) | `63557` | `19807` | `126453` |
| **54%** | [Hono](#hono) | `62864` | `15986` | `81002` |
| **22%** | [Carbon](#carbon) | `26127` | `7588` | `38121` |
| **15%** | [Express](#express) | `17927` | `4132` | `25559` |


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
    Reqs/sec     29705.36   14320.82  114900.19
    Latency        1.68ms     2.89ms   238.22ms
    HTTP codes:
      1xx - 0, 2xx - 91635, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8365
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8365
    Throughput:     6.19MB/s
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
    Reqs/sec     20287.51   14555.11  135339.06
    Latency        2.46ms     2.13ms   188.72ms
    HTTP codes:
      1xx - 0, 2xx - 88087, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11913
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11913
    Throughput:     5.12MB/s
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
    Reqs/sec     73880.69   20187.19  117357.00
    Latency      672.84us     1.05ms    84.07ms
    HTTP codes:
      1xx - 0, 2xx - 83747, 3xx - 0, 4xx - 0, 5xx - 0
      others - 16253
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 16253
    Throughput:    14.08MB/s
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
    Reqs/sec     65842.21   20389.37  133295.59
    Latency      757.19us     1.07ms    87.22ms
    HTTP codes:
      1xx - 0, 2xx - 92233, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7767
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7767
    Throughput:    13.72MB/s
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
    Reqs/sec    116801.42    4658.17  122397.25
    Latency      426.74us   170.44us     9.32ms
    HTTP codes:
      1xx - 0, 2xx - 93426, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6574
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6574
    Throughput:    15.50MB/s
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
    Reqs/sec     64386.41   20147.64  120673.95
    Latency      772.13us     1.16ms    93.99ms
    HTTP codes:
      1xx - 0, 2xx - 92979, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7021
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7021
    Throughput:    13.57MB/s
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
    Reqs/sec     72922.37   16108.52  125798.45
    Latency      684.53us     0.87ms    71.68ms
    HTTP codes:
      1xx - 0, 2xx - 96205, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3795
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3795
    Throughput:    16.05MB/s
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
    Reqs/sec    123977.79    3424.13  132048.91
    Latency      401.48us   137.01us     9.12ms
    HTTP codes:
      1xx - 0, 2xx - 95310, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4690
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4690
    Throughput:    18.70MB/s
  ```


