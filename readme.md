## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75099` | `2930` | `82749` |
| **83%** | [Hyper Express](#hyper-express) | `62568` | `3691` | `65326` |
| **33%** | [Node (Default)](#node-default) | `25152` | `8465` | `75977` |
| **31%** | [Fastify](#fastify) | `23169` | `6723` | `35040` |
| **28%** | [Hono](#hono) | `20839` | `5710` | `29992` |
| **26%** | [Koa](#koa) | `19744` | `8880` | `77020` |
| **11%** | [Carbon](#carbon) | `8185` | `1381` | `10212` |
| **9%** | [Express](#express) | `6390` | `1024` | `8307` |


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
    Reqs/sec      8982.98    5954.97   68984.04
    Latency        5.56ms     4.29ms   369.01ms
    HTTP codes:
      1xx - 0, 2xx - 91701, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8299
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8299
    Throughput:     1.87MB/s
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
    Reqs/sec      7064.62    5635.74   65620.18
    Latency        7.07ms     3.79ms   349.51ms
    HTTP codes:
      1xx - 0, 2xx - 90612, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9388
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9388
    Throughput:     1.83MB/s
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
    Reqs/sec     23614.30    7021.63   35748.45
    Latency        2.12ms     2.03ms   183.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.35MB/s
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
    Reqs/sec     20430.26    5577.32   29392.55
    Latency        2.45ms     1.96ms   179.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     62515.43    3383.44   65373.52
    Latency      797.51us    70.54us     2.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     19368.87   11039.47   81581.38
    Latency        2.58ms     2.47ms   215.87ms
    HTTP codes:
      1xx - 0, 2xx - 87545, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12455
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12455
    Throughput:     3.83MB/s
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
    Reqs/sec     25066.94    8355.90   71722.96
    Latency        1.99ms     2.01ms   171.65ms
    HTTP codes:
      1xx - 0, 2xx - 96508, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3492
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3492
    Throughput:     5.54MB/s
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
    Reqs/sec     75335.88    3023.41   87220.74
    Latency      661.12us   177.23us     8.47ms
    HTTP codes:
      1xx - 0, 2xx - 96840, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3160
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3160
    Throughput:    11.55MB/s
  ```


