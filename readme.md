## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73822` | `7490` | `90104` |
| **85%** | [Hyper Express](#hyper-express) | `63035` | `4144` | `69674` |
| **35%** | [Node (Default)](#node-default) | `25852` | `7971` | `51017` |
| **32%** | [Fastify](#fastify) | `23339` | `6931` | `35761` |
| **28%** | [Hono](#hono) | `20449` | `5659` | `29531` |
| **25%** | [Koa](#koa) | `18685` | `7770` | `57817` |
| **11%** | [Carbon](#carbon) | `7972` | `1352` | `10005` |
| **9%** | [Express](#express) | `6419` | `1038` | `8346` |


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
    Reqs/sec      8503.59    5111.66   54715.98
    Latency        5.87ms     4.45ms   381.79ms
    HTTP codes:
      1xx - 0, 2xx - 91301, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8699
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8699
    Throughput:     1.76MB/s
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
    Reqs/sec      6347.42    1024.12    8267.62
    Latency        7.87ms     3.74ms   358.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     23832.62    7377.30   36009.42
    Latency        2.10ms     2.11ms   188.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     21021.40    5949.70   29688.45
    Latency        2.38ms     2.06ms   184.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     63664.77    2904.86   65693.16
    Latency      782.94us    68.10us     4.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.04MB/s
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
    Reqs/sec     18424.08    6506.29   54328.25
    Latency        2.70ms     2.40ms   209.25ms
    HTTP codes:
      1xx - 0, 2xx - 93728, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6272
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6272
    Throughput:     3.91MB/s
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
    Reqs/sec     25800.17    7524.59   42199.06
    Latency        1.93ms     1.87ms   161.95ms
    HTTP codes:
      1xx - 0, 2xx - 97955, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2045
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2045
    Throughput:     5.79MB/s
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
    Reqs/sec     74113.16    6733.89   80467.24
    Latency      671.78us   195.61us    10.03ms
    HTTP codes:
      1xx - 0, 2xx - 97161, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2839
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2839
    Throughput:    11.39MB/s
  ```


