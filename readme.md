## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73537` | `5825` | `83651` |
| **86%** | [Hyper Express](#hyper-express) | `62954` | `3654` | `68004` |
| **33%** | [Node (Default)](#node-default) | `24269` | `6869` | `44224` |
| **32%** | [Fastify](#fastify) | `23808` | `6846` | `34974` |
| **29%** | [Hono](#hono) | `21304` | `5217` | `28828` |
| **25%** | [Koa](#koa) | `18276` | `6206` | `51994` |
| **11%** | [Carbon](#carbon) | `8272` | `1418` | `10447` |
| **9%** | [Express](#express) | `6750` | `1112` | `8655` |


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
    Reqs/sec      8882.13    4641.65   53911.56
    Latency        5.62ms     4.22ms   362.98ms
    HTTP codes:
      1xx - 0, 2xx - 92959, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7041
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7041
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
    Reqs/sec      6654.36    1091.72    8682.31
    Latency        7.51ms     3.47ms   336.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.90MB/s
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
    Reqs/sec     23640.48    6937.19   35496.04
    Latency        2.11ms     2.04ms   185.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.37MB/s
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
    Reqs/sec     20780.61    5705.02   29958.92
    Latency        2.40ms     1.97ms   178.47ms
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
    Reqs/sec     63735.84    3874.98   85889.70
    Latency      781.93us    69.45us     3.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.06MB/s
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
    Reqs/sec     18813.58    6749.05   59143.60
    Latency        2.65ms     2.38ms   207.15ms
    HTTP codes:
      1xx - 0, 2xx - 93862, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6138
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6138
    Throughput:     3.99MB/s
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
    Reqs/sec     26396.02    7626.11   48027.63
    Latency        1.89ms     1.87ms   161.06ms
    HTTP codes:
      1xx - 0, 2xx - 98232, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1768
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1768
    Throughput:     5.94MB/s
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
    Reqs/sec     73589.16    6630.90   86612.38
    Latency      676.97us   214.21us    10.14ms
    HTTP codes:
      1xx - 0, 2xx - 97483, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2517
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2517
    Throughput:    11.36MB/s
  ```


