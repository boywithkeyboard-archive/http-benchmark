## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71024` | `3349` | `78890` |
| **84%** | [Hyper Express](#hyper-express) | `59376` | `3817` | `67061` |
| **32%** | [Node (Default)](#node-default) | `23047` | `6219` | `58514` |
| **32%** | [Fastify](#fastify) | `22632` | `7354` | `37287` |
| **30%** | [Hono](#hono) | `20983` | `6530` | `30654` |
| **26%** | [Koa](#koa) | `18815` | `7859` | `60728` |
| **11%** | [Carbon](#carbon) | `8030` | `1447` | `10275` |
| **9%** | [Express](#express) | `6338` | `1119` | `8434` |


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
    Reqs/sec      8509.70    5805.11   76242.95
    Latency        5.87ms     4.39ms   382.03ms
    HTTP codes:
      1xx - 0, 2xx - 91520, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8480
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8480
    Throughput:     1.77MB/s
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
    Reqs/sec      6224.74    1079.89    8362.05
    Latency        8.03ms     3.87ms   369.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     23398.15    7415.09   37737.42
    Latency        2.14ms     2.14ms   190.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.31MB/s
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
    Reqs/sec     20506.31    5811.07   30575.37
    Latency        2.44ms     2.04ms   189.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     58928.75    3724.61   65383.44
    Latency      846.49us    89.14us     3.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.37MB/s
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
    Reqs/sec     18124.49    8164.68   71720.87
    Latency        2.75ms     2.44ms   213.79ms
    HTTP codes:
      1xx - 0, 2xx - 92194, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7806
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7806
    Throughput:     3.78MB/s
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
    Reqs/sec     23234.60    6226.48   60220.29
    Latency        2.15ms     1.99ms   170.81ms
    HTTP codes:
      1xx - 0, 2xx - 97433, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2567
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2567
    Throughput:     5.18MB/s
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
    Reqs/sec     71426.20    4474.66   87408.15
    Latency      697.78us   159.82us     7.13ms
    HTTP codes:
      1xx - 0, 2xx - 97140, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2860
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2860
    Throughput:    10.97MB/s
  ```


