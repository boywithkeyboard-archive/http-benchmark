## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74847` | `3672` | `93657` |
| **83%** | [Hyper Express](#hyper-express) | `62202` | `3943` | `65840` |
| **34%** | [Node (Default)](#node-default) | `25203` | `8879` | `81937` |
| **33%** | [Fastify](#fastify) | `24801` | `7935` | `36670` |
| **27%** | [Hono](#hono) | `20441` | `5815` | `30568` |
| **26%** | [Koa](#koa) | `19245` | `7854` | `69857` |
| **11%** | [Carbon](#carbon) | `8415` | `1463` | `10528` |
| **9%** | [Express](#express) | `6548` | `1110` | `8535` |


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
    Reqs/sec      8885.38    5527.23   69889.21
    Latency        5.61ms     4.23ms   365.12ms
    HTTP codes:
      1xx - 0, 2xx - 92437, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7563
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7563
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
    Reqs/sec      7139.16    6345.27   79669.22
    Latency        6.99ms     3.91ms   357.55ms
    HTTP codes:
      1xx - 0, 2xx - 89323, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10677
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10677
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
    Reqs/sec     23878.51    7373.11   36324.34
    Latency        2.09ms     2.01ms   177.85ms
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
    Reqs/sec     21900.97    6144.63   30817.69
    Latency        2.28ms     2.00ms   177.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.95MB/s
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
    Reqs/sec     63097.52    3181.38   66979.48
    Latency      790.10us    64.22us     2.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.96MB/s
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
    Reqs/sec     19297.95    8922.04   74181.50
    Latency        2.58ms     2.43ms   213.43ms
    HTTP codes:
      1xx - 0, 2xx - 91977, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8023
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8023
    Throughput:     4.02MB/s
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
    Reqs/sec     25042.98    7990.87   71010.08
    Latency        1.99ms     1.86ms   159.72ms
    HTTP codes:
      1xx - 0, 2xx - 96848, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3152
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3152
    Throughput:     5.55MB/s
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
    Reqs/sec     74898.13    3070.34   85827.47
    Latency      665.07us   122.29us     5.33ms
    HTTP codes:
      1xx - 0, 2xx - 97449, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2551
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2551
    Throughput:    11.55MB/s
  ```


