## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75252` | `1871` | `83058` |
| **86%** | [Hyper Express](#hyper-express) | `64622` | `3465` | `68602` |
| **33%** | [Fastify](#fastify) | `24841` | `7369` | `36446` |
| **33%** | [Node (Default)](#node-default) | `24837` | `7053` | `59456` |
| **28%** | [Hono](#hono) | `20823` | `5630` | `29631` |
| **26%** | [Koa](#koa) | `19537` | `8941` | `75176` |
| **11%** | [Carbon](#carbon) | `8458` | `1459` | `10555` |
| **9%** | [Express](#express) | `6421` | `1018` | `8449` |


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
    Reqs/sec      8708.47    4725.69   61883.03
    Latency        5.73ms     4.23ms   363.88ms
    HTTP codes:
      1xx - 0, 2xx - 93930, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6070
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6070
    Throughput:     1.86MB/s
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
    Reqs/sec      6403.92    1045.77    8496.55
    Latency        7.81ms     3.70ms   355.06ms
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
    Reqs/sec     24799.23    7695.88   37210.62
    Latency        2.02ms     1.97ms   176.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     20678.59    5308.73   28373.03
    Latency        2.42ms     2.02ms   182.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
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
    Reqs/sec     64014.79    3940.01   67919.67
    Latency      779.51us    67.18us     3.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.09MB/s
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
    Reqs/sec     19530.92    9641.04   75962.49
    Latency        2.56ms     2.32ms   202.54ms
    HTTP codes:
      1xx - 0, 2xx - 90496, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9504
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9504
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
    Reqs/sec     25547.82    8187.46   74357.81
    Latency        1.95ms     1.94ms   161.57ms
    HTTP codes:
      1xx - 0, 2xx - 96448, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3552
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3552
    Throughput:     5.65MB/s
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
    Reqs/sec     75139.76    2915.71   80918.19
    Latency      663.00us   132.23us     7.29ms
    HTTP codes:
      1xx - 0, 2xx - 97357, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2643
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2643
    Throughput:    11.58MB/s
  ```


