## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72922` | `2536` | `83497` |
| **80%** | [Hyper Express](#hyper-express) | `58626` | `2951` | `60889` |
| **35%** | [Node (Default)](#node-default) | `25534` | `9617` | `79739` |
| **31%** | [Fastify](#fastify) | `22648` | `7127` | `35174` |
| **27%** | [Hono](#hono) | `19758` | `6061` | `29996` |
| **25%** | [Koa](#koa) | `18119` | `7224` | `59329` |
| **11%** | [Carbon](#carbon) | `8045` | `1486` | `10213` |
| **9%** | [Express](#express) | `6209` | `1123` | `8301` |


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
    Reqs/sec      8784.74    7180.97   80434.53
    Latency        5.68ms     4.42ms   389.41ms
    HTTP codes:
      1xx - 0, 2xx - 88970, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11030
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11030
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
    Reqs/sec      6166.52    1072.58    8254.16
    Latency        8.10ms     3.69ms   352.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23557.56    7857.61   35937.09
    Latency        2.12ms     2.41ms   210.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.34MB/s
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
    Reqs/sec     19351.63    5698.86   29216.18
    Latency        2.58ms     2.04ms   184.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.37MB/s
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
    Reqs/sec     59627.64    3279.95   62705.98
    Latency      836.27us    77.10us     3.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.47MB/s
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
    Reqs/sec     18049.75    8038.04   66659.53
    Latency        2.76ms     2.46ms   216.01ms
    HTTP codes:
      1xx - 0, 2xx - 93085, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6915
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6915
    Throughput:     3.80MB/s
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
    Reqs/sec     23283.32    7375.38   78200.54
    Latency        2.14ms     2.02ms   166.29ms
    HTTP codes:
      1xx - 0, 2xx - 96458, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3542
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3542
    Throughput:     5.15MB/s
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
    Reqs/sec     72880.39    2586.80   82596.94
    Latency      683.09us   199.62us    10.02ms
    HTTP codes:
      1xx - 0, 2xx - 96384, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3616
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3616
    Throughput:    11.11MB/s
  ```


