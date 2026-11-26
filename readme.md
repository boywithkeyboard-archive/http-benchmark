## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `197400` | `7200` | `206447` |
| **85%** | [Hyper Express](#hyper-express) | `167042` | `13113` | `174930` |
| **38%** | [Node (Default)](#node-default) | `74578` | `16453` | `146624` |
| **37%** | [Fastify](#fastify) | `73619` | `16938` | `93757` |
| **34%** | [Hono](#hono) | `67308` | `16193` | `84585` |
| **34%** | [Koa](#koa) | `66890` | `25455` | `180639` |
| **15%** | [Carbon](#carbon) | `28773` | `6483` | `35010` |
| **9%** | [Express](#express) | `18253` | `3564` | `23299` |


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
    Reqs/sec     31449.34   21450.13  167642.64
    Latency        1.58ms     2.54ms   201.91ms
    HTTP codes:
      1xx - 0, 2xx - 86343, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13657
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13657
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
    Reqs/sec     20700.13   18018.19  169968.57
    Latency        2.41ms     2.49ms   212.34ms
    HTTP codes:
      1xx - 0, 2xx - 86002, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13998
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13998
    Throughput:     5.10MB/s
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
    Reqs/sec     86115.61   36652.74  177548.91
    Latency      576.22us     0.88ms    67.24ms
    HTTP codes:
      1xx - 0, 2xx - 70674, 3xx - 0, 4xx - 0, 5xx - 0
      others - 29326
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 29324
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:    13.85MB/s
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
    Reqs/sec     72539.40   25766.18  171597.56
    Latency      684.05us     0.92ms    57.62ms
    HTTP codes:
      1xx - 0, 2xx - 88671, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11329
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11329
    Throughput:    14.61MB/s
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
    Reqs/sec    166189.38   14584.28  177875.91
    Latency      299.23us   158.66us     5.76ms
    HTTP codes:
      1xx - 0, 2xx - 89608, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10392
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10392
    Throughput:    21.11MB/s
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
    Reqs/sec     65967.42   21727.55  143637.29
    Latency      754.56us     1.07ms    87.15ms
    HTTP codes:
      1xx - 0, 2xx - 89219, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10781
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10781
    Throughput:    13.32MB/s
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
    Reqs/sec     78088.53   23912.42  184999.72
    Latency      638.33us   681.50us    47.33ms
    HTTP codes:
      1xx - 0, 2xx - 92374, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7626
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7626
    Throughput:    16.50MB/s
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
    Reqs/sec    193809.37   11811.79  215851.22
    Latency      254.78us   133.37us     6.43ms
    HTTP codes:
      1xx - 0, 2xx - 92111, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7889
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7889
    Throughput:    28.28MB/s
  ```


