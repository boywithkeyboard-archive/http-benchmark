## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68307` | `4523` | `80437` |
| **84%** | [Hyper Express](#hyper-express) | `57127` | `3653` | `60374` |
| **31%** | [Fastify](#fastify) | `21188` | `5434` | `36458` |
| **30%** | [Hono](#hono) | `20369` | `6239` | `30547` |
| **30%** | [Node (Default)](#node-default) | `20180` | `6161` | `75888` |
| **29%** | [Koa](#koa) | `19702` | `9429` | `73855` |
| **11%** | [Carbon](#carbon) | `7288` | `1258` | `10303` |
| **9%** | [Express](#express) | `6055` | `1009` | `8534` |


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
    Reqs/sec      8204.19    6400.68   67106.22
    Latency        6.08ms     4.80ms   406.13ms
    HTTP codes:
      1xx - 0, 2xx - 89324, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10676
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10676
    Throughput:     1.66MB/s
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
    Reqs/sec      6173.84    1110.16    8142.12
    Latency        8.09ms     3.87ms   376.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     21493.49    6180.29   36627.07
    Latency        2.33ms     2.08ms   186.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.87MB/s
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
    Reqs/sec     20930.68    6704.40   31946.85
    Latency        2.39ms     2.27ms   198.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     57419.23    3039.51   65625.32
    Latency        0.87ms    99.02us     3.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.16MB/s
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
    Reqs/sec     18841.91    8212.43   60947.02
    Latency        2.65ms     2.50ms   215.63ms
    HTTP codes:
      1xx - 0, 2xx - 93307, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6693
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6693
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
    Reqs/sec     20450.73    5849.44   63249.50
    Latency        2.44ms     1.99ms   171.22ms
    HTTP codes:
      1xx - 0, 2xx - 96304, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3696
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3696
    Throughput:     4.51MB/s
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
    Reqs/sec     68783.43    3607.73   79568.07
    Latency      722.93us   228.95us    13.95ms
    HTTP codes:
      1xx - 0, 2xx - 96348, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3652
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3652
    Throughput:    10.49MB/s
  ```


