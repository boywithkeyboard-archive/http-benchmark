## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71276` | `4627` | `80322` |
| **86%** | [Hyper Express](#hyper-express) | `61057` | `4642` | `70769` |
| **30%** | [Hono](#hono) | `21532` | `6335` | `29430` |
| **30%** | [Node (Default)](#node-default) | `21369` | `6249` | `65808` |
| **29%** | [Fastify](#fastify) | `20449` | `4755` | `33436` |
| **27%** | [Koa](#koa) | `19402` | `8314` | `62076` |
| **10%** | [Carbon](#carbon) | `7304` | `1263` | `10017` |
| **8%** | [Express](#express) | `5894` | `992` | `8474` |


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
    Reqs/sec      8058.09    6379.90   70012.21
    Latency        6.20ms     4.62ms   395.13ms
    HTTP codes:
      1xx - 0, 2xx - 89786, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10214
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10214
    Throughput:     1.64MB/s
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
    Reqs/sec      6008.24    1035.91    9341.93
    Latency        8.32ms     3.99ms   380.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.72MB/s
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
    Reqs/sec     20950.06    6018.14   36721.94
    Latency        2.39ms     2.19ms   193.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     21686.29    6447.76   29466.31
    Latency        2.30ms     2.21ms   194.71ms
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
    Reqs/sec     61328.10    4340.03   70822.48
    Latency      813.10us    96.32us     4.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     20401.70    8841.46   65436.07
    Latency        2.44ms     2.53ms   216.88ms
    HTTP codes:
      1xx - 0, 2xx - 92365, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7635
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7635
    Throughput:     4.26MB/s
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
    Reqs/sec     21228.95    5770.22   66446.14
    Latency        2.35ms     2.14ms   189.56ms
    HTTP codes:
      1xx - 0, 2xx - 96711, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3289
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3289
    Throughput:     4.71MB/s
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
    Reqs/sec     73358.37    4048.84   81094.77
    Latency      678.94us   198.42us    10.35ms
    HTTP codes:
      1xx - 0, 2xx - 94620, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5380
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5380
    Throughput:    10.98MB/s
  ```


