## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73378` | `6022` | `78489` |
| **84%** | [Hyper Express](#hyper-express) | `61508` | `3271` | `64319` |
| **37%** | [Node (Default)](#node-default) | `26815` | `8176` | `59937` |
| **33%** | [Fastify](#fastify) | `23947` | `7256` | `35734` |
| **28%** | [Hono](#hono) | `20679` | `5716` | `29304` |
| **26%** | [Koa](#koa) | `19241` | `7027` | `55271` |
| **11%** | [Carbon](#carbon) | `7959` | `1273` | `10112` |
| **9%** | [Express](#express) | `6539` | `1062` | `8332` |


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
    Reqs/sec      9040.83    5027.47   57509.53
    Latency        5.52ms     4.24ms   365.83ms
    HTTP codes:
      1xx - 0, 2xx - 92348, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7652
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7652
    Throughput:     1.90MB/s
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
    Reqs/sec      6847.21    4076.80   54749.85
    Latency        7.30ms     3.89ms   357.42ms
    HTTP codes:
      1xx - 0, 2xx - 93079, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6921
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6921
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
    Reqs/sec     24327.68    7459.51   36116.28
    Latency        2.05ms     2.06ms   185.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     20303.60    5562.22   29056.51
    Latency        2.46ms     1.96ms   180.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     61802.95    3376.75   65061.59
    Latency      806.67us    70.00us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.78MB/s
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
    Reqs/sec     18387.84    6150.19   49847.92
    Latency        2.71ms     2.29ms   205.42ms
    HTTP codes:
      1xx - 0, 2xx - 95010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4990
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
    Reqs/sec     26668.01    8090.22   61679.69
    Latency        1.87ms     1.79ms   156.43ms
    HTTP codes:
      1xx - 0, 2xx - 97027, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2973
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2973
    Throughput:     5.93MB/s
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
    Reqs/sec     72699.17    4298.25   77734.67
    Latency      685.42us   165.57us     9.66ms
    HTTP codes:
      1xx - 0, 2xx - 97400, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2600
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2600
    Throughput:    11.20MB/s
  ```


