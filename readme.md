## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71785` | `4001` | `84577` |
| **83%** | [Hyper Express](#hyper-express) | `59878` | `3738` | `67798` |
| **33%** | [Node (Default)](#node-default) | `23686` | `6192` | `57131` |
| **32%** | [Fastify](#fastify) | `23161` | `6785` | `37975` |
| **30%** | [Hono](#hono) | `21499` | `6428` | `30515` |
| **27%** | [Koa](#koa) | `19443` | `8463` | `71974` |
| **11%** | [Carbon](#carbon) | `8198` | `1438` | `10503` |
| **9%** | [Express](#express) | `6345` | `1022` | `8339` |


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
    Reqs/sec      8842.08    5583.50   67644.02
    Latency        5.64ms     4.30ms   369.93ms
    HTTP codes:
      1xx - 0, 2xx - 92108, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7892
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7892
    Throughput:     1.85MB/s
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
    Reqs/sec      6608.69    1150.79    8394.07
    Latency        7.56ms     3.69ms   352.32ms
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
    Reqs/sec     22782.45    6015.62   38180.45
    Latency        2.19ms     2.04ms   184.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.17MB/s
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
    Reqs/sec     21186.48    5885.73   30428.43
    Latency        2.36ms     1.96ms   176.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     60638.06    3664.80   67345.85
    Latency      822.53us    81.90us     3.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.61MB/s
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
    Reqs/sec     18950.85    8257.92   70474.99
    Latency        2.63ms     2.36ms   207.77ms
    HTTP codes:
      1xx - 0, 2xx - 92842, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7158
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7158
    Throughput:     3.98MB/s
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
    Reqs/sec     24365.30    6968.96   75091.17
    Latency        2.05ms     1.97ms   171.16ms
    HTTP codes:
      1xx - 0, 2xx - 96718, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3282
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3282
    Throughput:     5.40MB/s
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
    Reqs/sec     71607.66    3225.56   78078.14
    Latency      695.78us   131.91us     5.24ms
    HTTP codes:
      1xx - 0, 2xx - 97463, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2537
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2537
    Throughput:    11.05MB/s
  ```


