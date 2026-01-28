## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75333` | `3183` | `79736` |
| **84%** | [Hyper Express](#hyper-express) | `63261` | `3372` | `70665` |
| **36%** | [Node (Default)](#node-default) | `27336` | `8837` | `77194` |
| **31%** | [Fastify](#fastify) | `23534` | `6825` | `36230` |
| **29%** | [Hono](#hono) | `21821` | `6406` | `31569` |
| **26%** | [Koa](#koa) | `19834` | `8899` | `72481` |
| **11%** | [Carbon](#carbon) | `8472` | `1498` | `10463` |
| **9%** | [Express](#express) | `6538` | `1136` | `8492` |


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
    Reqs/sec      9086.03    5654.91   73153.84
    Latency        5.49ms     4.21ms   362.71ms
    HTTP codes:
      1xx - 0, 2xx - 92620, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7380
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7380
    Throughput:     1.91MB/s
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
    Reqs/sec      7236.76    5862.23   77074.29
    Latency        6.90ms     3.82ms   351.51ms
    HTTP codes:
      1xx - 0, 2xx - 90503, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9497
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9497
    Throughput:     1.88MB/s
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
    Reqs/sec     23916.21    6868.63   37116.00
    Latency        2.09ms     1.95ms   177.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     22028.58    6155.06   30941.63
    Latency        2.27ms     1.98ms   178.32ms
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
    Reqs/sec     63118.92    3826.97   65500.27
    Latency      789.16us    75.56us     3.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.97MB/s
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
    Reqs/sec     19293.50    9651.05   81591.80
    Latency        2.58ms     2.33ms   204.23ms
    HTTP codes:
      1xx - 0, 2xx - 90326, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9674
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9674
    Throughput:     3.94MB/s
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
    Reqs/sec     27949.89   10534.18   83686.17
    Latency        1.79ms     1.88ms   163.60ms
    HTTP codes:
      1xx - 0, 2xx - 95501, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4499
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4499
    Throughput:     6.11MB/s
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
    Reqs/sec     75550.78    1936.22   77698.39
    Latency      659.06us   131.24us     6.20ms
    HTTP codes:
      1xx - 0, 2xx - 97244, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2756
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2756
    Throughput:    11.63MB/s
  ```


