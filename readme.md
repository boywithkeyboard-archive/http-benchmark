## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73958` | `2503` | `82483` |
| **84%** | [Hyper Express](#hyper-express) | `61992` | `2765` | `64371` |
| **33%** | [Node (Default)](#node-default) | `24532` | `7210` | `72075` |
| **32%** | [Fastify](#fastify) | `24019` | `7144` | `35292` |
| **30%** | [Hono](#hono) | `21872` | `6462` | `30480` |
| **26%** | [Koa](#koa) | `19311` | `8940` | `77251` |
| **11%** | [Carbon](#carbon) | `8182` | `1404` | `10350` |
| **8%** | [Express](#express) | `6217` | `976` | `8242` |


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
    Reqs/sec      8970.19    5954.04   73881.23
    Latency        5.56ms     4.37ms   373.99ms
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
    Reqs/sec      6714.49    5117.64   66355.82
    Latency        7.44ms     3.96ms   363.99ms
    HTTP codes:
      1xx - 0, 2xx - 91665, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8335
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8335
    Throughput:     1.76MB/s
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
    Reqs/sec     24490.80    7514.86   35563.02
    Latency        2.04ms     2.05ms   181.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.56MB/s
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
    Reqs/sec     20763.57    5802.29   30737.61
    Latency        2.41ms     2.14ms   191.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     61972.54    3196.68   65456.78
    Latency      804.71us    70.40us     2.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     19483.99    8844.78   73975.70
    Latency        2.56ms     2.42ms   214.81ms
    HTTP codes:
      1xx - 0, 2xx - 92048, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7952
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7952
    Throughput:     4.06MB/s
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
    Reqs/sec     26110.24    8161.50   61355.11
    Latency        1.91ms     1.93ms   168.84ms
    HTTP codes:
      1xx - 0, 2xx - 97495, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2505
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2505
    Throughput:     5.82MB/s
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
    Reqs/sec     74370.87    1678.64   81406.98
    Latency      669.14us   166.11us     7.42ms
    HTTP codes:
      1xx - 0, 2xx - 95312, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4688
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4688
    Throughput:    11.23MB/s
  ```


