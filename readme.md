## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74186` | `3308` | `86807` |
| **84%** | [Hyper Express](#hyper-express) | `62252` | `4316` | `68670` |
| **30%** | [Hono](#hono) | `22478` | `6488` | `30818` |
| **29%** | [Node (Default)](#node-default) | `21743` | `6191` | `65053` |
| **29%** | [Fastify](#fastify) | `21743` | `5639` | `37148` |
| **25%** | [Koa](#koa) | `18653` | `8345` | `65617` |
| **10%** | [Carbon](#carbon) | `7544` | `1269` | `10119` |
| **8%** | [Express](#express) | `6225` | `997` | `8347` |


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
    Reqs/sec      8302.34    5792.56   79527.94
    Latency        6.01ms     4.41ms   381.97ms
    HTTP codes:
      1xx - 0, 2xx - 91625, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8375
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8375
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
    Reqs/sec      6301.91    1052.60    8362.24
    Latency        7.93ms     3.91ms   373.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     23190.69    7399.86   37489.40
    Latency        2.15ms     1.99ms   181.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.26MB/s
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
    Reqs/sec     22915.36    6736.82   31446.17
    Latency        2.18ms     1.99ms   180.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.17MB/s
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
    Reqs/sec     62055.84    3587.75   67385.66
    Latency      803.95us    81.51us     3.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     19175.27    8610.15   64970.78
    Latency        2.60ms     2.53ms   220.30ms
    HTTP codes:
      1xx - 0, 2xx - 92290, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7710
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7710
    Throughput:     4.00MB/s
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
    Reqs/sec     22388.46    7715.17   77205.64
    Latency        2.23ms     1.94ms   168.24ms
    HTTP codes:
      1xx - 0, 2xx - 95478, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4522
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4522
    Throughput:     4.90MB/s
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
    Reqs/sec     73719.75    5224.43   92217.13
    Latency      676.23us   149.87us     6.54ms
    HTTP codes:
      1xx - 0, 2xx - 97295, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2705
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2705
    Throughput:    11.35MB/s
  ```


