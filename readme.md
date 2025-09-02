## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75786` | `3301` | `85405` |
| **83%** | [Hyper Express](#hyper-express) | `63063` | `3872` | `66707` |
| **33%** | [Node (Default)](#node-default) | `24913` | `8142` | `81407` |
| **31%** | [Fastify](#fastify) | `23828` | `6911` | `37531` |
| **28%** | [Hono](#hono) | `21303` | `5965` | `31103` |
| **26%** | [Koa](#koa) | `19435` | `9457` | `87439` |
| **11%** | [Carbon](#carbon) | `8447` | `1480` | `10503` |
| **9%** | [Express](#express) | `6527` | `1113` | `8530` |


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
    Reqs/sec      9072.91    6145.39   79299.14
    Latency        5.50ms     4.28ms   368.85ms
    HTTP codes:
      1xx - 0, 2xx - 91572, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8428
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8428
    Throughput:     1.89MB/s
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
    Reqs/sec      7215.90    6418.74   83927.99
    Latency        6.90ms     3.90ms   353.56ms
    HTTP codes:
      1xx - 0, 2xx - 89086, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10914
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10914
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
    Reqs/sec     24820.65    7745.46   37775.24
    Latency        2.01ms     2.04ms   184.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.63MB/s
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
    Reqs/sec     21698.30    6247.87   31145.63
    Latency        2.30ms     1.93ms   175.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.90MB/s
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
    Reqs/sec     62963.74    3748.67   65898.17
    Latency      791.27us    73.70us     2.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     18562.91    8404.31   71557.67
    Latency        2.69ms     2.42ms   211.29ms
    HTTP codes:
      1xx - 0, 2xx - 92283, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7717
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7717
    Throughput:     3.87MB/s
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
    Reqs/sec     24294.64    7257.40   66287.07
    Latency        2.05ms     1.91ms   166.35ms
    HTTP codes:
      1xx - 0, 2xx - 97355, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2645
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2641
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 4
    Throughput:     5.42MB/s
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
    Reqs/sec     75928.16    2758.69   81148.05
    Latency      655.44us   169.96us     9.27ms
    HTTP codes:
      1xx - 0, 2xx - 96444, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3556
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3556
    Throughput:    11.59MB/s
  ```


