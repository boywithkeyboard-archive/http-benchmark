## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `81507` | `3871` | `86310` |
| **87%** | [Hyper Express](#hyper-express) | `71295` | `2848` | `73889` |
| **43%** | [Node (Default)](#node-default) | `35246` | `10495` | `75058` |
| **40%** | [Fastify](#fastify) | `32868` | `10062` | `52404` |
| **37%** | [Koa](#koa) | `29801` | `12960` | `86425` |
| **36%** | [Hono](#hono) | `29514` | `7762` | `45732` |
| **12%** | [Carbon](#carbon) | `9491` | `2422` | `13531` |
| **9%** | [Express](#express) | `7449` | `1662` | `11017` |


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
    Reqs/sec      9997.01    6938.74   78301.38
    Latency        4.99ms     4.19ms   360.51ms
    HTTP codes:
      1xx - 0, 2xx - 91474, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8526
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8526
    Throughput:     2.08MB/s
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
    Reqs/sec      8392.27    7462.67   87504.04
    Latency        5.95ms     3.85ms   351.40ms
    HTTP codes:
      1xx - 0, 2xx - 88789, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11211
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11211
    Throughput:     2.13MB/s
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
    Reqs/sec     36545.85   12606.09   53496.29
    Latency        1.37ms     1.93ms   168.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.30MB/s
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
    Reqs/sec     29196.78    8198.41   46821.12
    Latency        1.71ms     2.01ms   178.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.59MB/s
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
    Reqs/sec     70531.23    3847.17   73756.65
    Latency      707.17us    70.47us     3.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.02MB/s
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
    Reqs/sec     30617.89   13552.65   84815.88
    Latency        1.63ms     2.30ms   199.93ms
    HTTP codes:
      1xx - 0, 2xx - 91795, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8205
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8205
    Throughput:     6.35MB/s
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
    Reqs/sec     36581.77   12074.30   89468.47
    Latency        1.36ms     1.79ms   149.24ms
    HTTP codes:
      1xx - 0, 2xx - 94946, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5054
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5054
    Throughput:     7.95MB/s
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
    Reqs/sec     82071.73    2836.91   89002.12
    Latency      606.23us   173.61us    10.20ms
    HTTP codes:
      1xx - 0, 2xx - 94990, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5010
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5010
    Throughput:    12.34MB/s
  ```


