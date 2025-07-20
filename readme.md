## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73877` | `3322` | `87268` |
| **85%** | [Hyper Express](#hyper-express) | `62538` | `3682` | `65237` |
| **35%** | [Node (Default)](#node-default) | `25817` | `8274` | `77887` |
| **32%** | [Fastify](#fastify) | `23481` | `6854` | `36207` |
| **27%** | [Hono](#hono) | `20294` | `6118` | `29893` |
| **26%** | [Koa](#koa) | `19382` | `8484` | `73281` |
| **11%** | [Carbon](#carbon) | `8215` | `1463` | `10268` |
| **9%** | [Express](#express) | `6386` | `1076` | `8368` |


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
    Reqs/sec      9100.09    7107.47   77492.13
    Latency        5.49ms     4.38ms   373.96ms
    HTTP codes:
      1xx - 0, 2xx - 89412, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10588
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10588
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
    Reqs/sec      6860.53    5978.22   75499.63
    Latency        7.27ms     4.05ms   373.00ms
    HTTP codes:
      1xx - 0, 2xx - 89486, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10514
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10514
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
    Reqs/sec     24665.30    7793.16   35631.63
    Latency        2.03ms     2.15ms   191.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.59MB/s
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
    Reqs/sec     21477.98    6195.04   29617.85
    Latency        2.33ms     2.08ms   185.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     62489.58    3582.77   66108.19
    Latency      797.93us    71.50us     2.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     19405.18   10490.48   79309.04
    Latency        2.57ms     2.45ms   215.73ms
    HTTP codes:
      1xx - 0, 2xx - 88777, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11223
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11223
    Throughput:     3.90MB/s
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
    Reqs/sec     25386.33    7814.16   68082.12
    Latency        1.97ms     2.01ms   172.51ms
    HTTP codes:
      1xx - 0, 2xx - 97034, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2966
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2966
    Throughput:     5.64MB/s
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
    Reqs/sec     74044.19    3207.03   77624.57
    Latency      672.66us   162.15us     7.54ms
    HTTP codes:
      1xx - 0, 2xx - 96805, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3195
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3195
    Throughput:    11.34MB/s
  ```


