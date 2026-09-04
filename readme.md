## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73094` | `4107` | `83642` |
| **84%** | [Hyper Express](#hyper-express) | `61309` | `3649` | `69287` |
| **32%** | [Node (Default)](#node-default) | `23369` | `6555` | `68583` |
| **30%** | [Fastify](#fastify) | `21574` | `5704` | `36867` |
| **29%** | [Hono](#hono) | `21326` | `6420` | `31127` |
| **25%** | [Koa](#koa) | `18360` | `7686` | `65288` |
| **11%** | [Carbon](#carbon) | `7708` | `1276` | `10547` |
| **9%** | [Express](#express) | `6442` | `1082` | `8486` |


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
    Reqs/sec      8249.17    5498.61   72359.08
    Latency        6.04ms     4.37ms   374.89ms
    HTTP codes:
      1xx - 0, 2xx - 91880, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8120
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8120
    Throughput:     1.73MB/s
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
    Reqs/sec      6392.57    1068.46    8394.46
    Latency        7.82ms     3.81ms   365.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     21721.23    6079.13   37048.14
    Latency        2.30ms     2.11ms   187.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.93MB/s
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
    Reqs/sec     22268.84    6693.41   31212.21
    Latency        2.24ms     2.30ms   198.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.03MB/s
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
    Reqs/sec     61326.13    3711.34   65896.53
    Latency      813.45us    90.29us     3.10ms
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
    Reqs/sec     19201.90    9275.24   75247.23
    Latency        2.60ms     2.32ms   205.21ms
    HTTP codes:
      1xx - 0, 2xx - 90841, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9159
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9159
    Throughput:     3.95MB/s
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
    Reqs/sec     23372.23    6343.05   64354.18
    Latency        2.14ms     1.92ms   169.28ms
    HTTP codes:
      1xx - 0, 2xx - 96491, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3509
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3509
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
    Reqs/sec     71710.49    3862.85   84105.02
    Latency      694.75us   142.72us     5.13ms
    HTTP codes:
      1xx - 0, 2xx - 96983, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3017
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3017
    Throughput:    11.01MB/s
  ```


