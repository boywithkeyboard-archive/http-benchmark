## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74746` | `6982` | `90365` |
| **84%** | [Hyper Express](#hyper-express) | `62479` | `2687` | `64949` |
| **35%** | [Node (Default)](#node-default) | `26265` | `7297` | `50771` |
| **32%** | [Fastify](#fastify) | `24144` | `7170` | `36215` |
| **27%** | [Hono](#hono) | `20408` | `5362` | `29579` |
| **25%** | [Koa](#koa) | `18394` | `6456` | `50044` |
| **11%** | [Carbon](#carbon) | `8413` | `1455` | `10442` |
| **9%** | [Express](#express) | `6643` | `1106` | `8413` |


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
    Reqs/sec      8955.49    3989.80   48527.29
    Latency        5.56ms     4.39ms   372.85ms
    HTTP codes:
      1xx - 0, 2xx - 93247, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6753
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6753
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
    Reqs/sec      7055.21    4420.09   50286.79
    Latency        7.08ms     3.93ms   358.13ms
    HTTP codes:
      1xx - 0, 2xx - 92080, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7920
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7920
    Throughput:     1.86MB/s
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
    Reqs/sec     24434.45    7382.67   36036.36
    Latency        2.04ms     2.00ms   181.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.55MB/s
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
    Reqs/sec     20899.18    5873.03   30110.84
    Latency        2.39ms     1.95ms   179.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     62859.16    4137.63   70749.97
    Latency      793.23us    75.66us     3.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     18491.28    6369.04   48592.67
    Latency        2.70ms     2.35ms   206.58ms
    HTTP codes:
      1xx - 0, 2xx - 94914, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5086
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5086
    Throughput:     3.97MB/s
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
    Reqs/sec     25899.49    7484.37   49718.30
    Latency        1.93ms     1.97ms   170.44ms
    HTTP codes:
      1xx - 0, 2xx - 97893, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2107
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2107
    Throughput:     5.81MB/s
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
    Reqs/sec     74543.50    4973.87   83636.20
    Latency      668.85us   163.22us     7.49ms
    HTTP codes:
      1xx - 0, 2xx - 96926, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3074
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3074
    Throughput:    11.42MB/s
  ```


