## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71520` | `3394` | `80562` |
| **84%** | [Hyper Express](#hyper-express) | `60306` | `3957` | `66544` |
| **33%** | [Node (Default)](#node-default) | `23421` | `6082` | `59052` |
| **32%** | [Fastify](#fastify) | `22662` | `6034` | `36702` |
| **30%** | [Hono](#hono) | `21432` | `6458` | `31271` |
| **26%** | [Koa](#koa) | `18807` | `9812` | `78257` |
| **11%** | [Carbon](#carbon) | `8106` | `1475` | `11765` |
| **9%** | [Express](#express) | `6325` | `1096` | `8405` |


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
    Reqs/sec      8701.86    5382.59   66661.78
    Latency        5.73ms     4.44ms   383.19ms
    HTTP codes:
      1xx - 0, 2xx - 92198, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7802
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7802
    Throughput:     1.82MB/s
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
    Reqs/sec      6268.64    1083.22    8333.57
    Latency        7.97ms     3.76ms   360.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     21842.71    6034.36   36546.75
    Latency        2.29ms     2.13ms   190.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.95MB/s
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
    Reqs/sec     20870.33    6482.38   30099.22
    Latency        2.39ms     2.11ms   190.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     59061.08    3597.76   70135.19
    Latency      844.06us    85.03us     4.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.39MB/s
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
    Reqs/sec     18255.10    8175.64   63651.96
    Latency        2.73ms     2.53ms   226.78ms
    HTTP codes:
      1xx - 0, 2xx - 91860, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8140
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8140
    Throughput:     3.79MB/s
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
    Reqs/sec     23402.59    6228.21   59077.10
    Latency        2.13ms     1.96ms   169.48ms
    HTTP codes:
      1xx - 0, 2xx - 97369, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2631
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2631
    Throughput:     5.22MB/s
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
    Reqs/sec     72016.05    3898.81   80110.41
    Latency      690.70us   192.74us    10.24ms
    HTTP codes:
      1xx - 0, 2xx - 96502, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3498
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3498
    Throughput:    11.00MB/s
  ```


