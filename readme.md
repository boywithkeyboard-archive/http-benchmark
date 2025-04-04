## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73993` | `6835` | `91162` |
| **85%** | [Hyper Express](#hyper-express) | `63074` | `3160` | `67656` |
| **36%** | [Node (Default)](#node-default) | `26749` | `7972` | `56601` |
| **33%** | [Fastify](#fastify) | `24078` | `6959` | `36951` |
| **28%** | [Hono](#hono) | `20812` | `5745` | `29955` |
| **25%** | [Koa](#koa) | `18733` | `7814` | `61634` |
| **11%** | [Carbon](#carbon) | `8177` | `1379` | `10360` |
| **9%** | [Express](#express) | `6593` | `1051` | `8565` |


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
    Reqs/sec      8849.72    4718.55   54237.03
    Latency        5.64ms     4.18ms   362.12ms
    HTTP codes:
      1xx - 0, 2xx - 93075, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6925
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6925
    Throughput:     1.87MB/s
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
    Reqs/sec      7209.00    4877.07   59273.23
    Latency        6.93ms     3.89ms   354.44ms
    HTTP codes:
      1xx - 0, 2xx - 91244, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8756
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8756
    Throughput:     1.88MB/s
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
    Reqs/sec     24819.65    7362.34   37558.71
    Latency        2.01ms     1.96ms   175.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.63MB/s
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
    Reqs/sec     21245.33    5923.52   30821.34
    Latency        2.35ms     2.07ms   186.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     62587.70    3561.81   67141.40
    Latency      796.80us    70.24us     4.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     18873.67    6133.14   53268.92
    Latency        2.63ms     2.41ms   206.05ms
    HTTP codes:
      1xx - 0, 2xx - 94865, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5135
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5135
    Throughput:     4.06MB/s
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
    Reqs/sec     27630.89    8180.97   49294.42
    Latency        1.81ms     1.87ms   161.49ms
    HTTP codes:
      1xx - 0, 2xx - 97882, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2118
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2118
    Throughput:     6.18MB/s
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
    Reqs/sec     75319.04    5769.46   89393.32
    Latency      662.57us   151.35us     6.12ms
    HTTP codes:
      1xx - 0, 2xx - 97095, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2905
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2905
    Throughput:    11.56MB/s
  ```


