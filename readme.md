## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74180` | `4762` | `83723` |
| **84%** | [Hyper Express](#hyper-express) | `62153` | `2580` | `66437` |
| **36%** | [Node (Default)](#node-default) | `26842` | `8262` | `57415` |
| **32%** | [Fastify](#fastify) | `24075` | `6654` | `36998` |
| **30%** | [Hono](#hono) | `21949` | `6212` | `30947` |
| **26%** | [Koa](#koa) | `19024` | `7369` | `54118` |
| **11%** | [Carbon](#carbon) | `8379` | `1504` | `10349` |
| **9%** | [Express](#express) | `6387` | `1022` | `8415` |


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
    Reqs/sec      8592.94    4068.50   50512.69
    Latency        5.80ms     4.29ms   370.56ms
    HTTP codes:
      1xx - 0, 2xx - 94363, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5637
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5637
    Throughput:     1.84MB/s
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
    Reqs/sec      6596.87    1139.42    8491.49
    Latency        7.58ms     3.67ms   355.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     23995.88    7024.90   36928.46
    Latency        2.08ms     2.03ms   179.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     21389.18    5780.85   30703.08
    Latency        2.34ms     1.97ms   179.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     62249.56    3566.65   64980.34
    Latency      800.78us    70.53us     3.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     19015.21    6901.34   54677.62
    Latency        2.63ms     2.38ms   210.96ms
    HTTP codes:
      1xx - 0, 2xx - 93719, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6281
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6281
    Throughput:     4.03MB/s
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
    Reqs/sec     26094.17    7450.74   55701.67
    Latency        1.91ms     1.94ms   168.08ms
    HTTP codes:
      1xx - 0, 2xx - 97022, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2978
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2978
    Throughput:     5.80MB/s
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
    Reqs/sec     75559.36    4810.26   87243.43
    Latency      658.88us   141.83us     6.19ms
    HTTP codes:
      1xx - 0, 2xx - 97357, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2643
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2643
    Throughput:    11.63MB/s
  ```


