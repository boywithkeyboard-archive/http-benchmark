## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75470` | `2923` | `83128` |
| **82%** | [Hyper Express](#hyper-express) | `61952` | `3635` | `65725` |
| **32%** | [Node (Default)](#node-default) | `23784` | `7049` | `78475` |
| **28%** | [Fastify](#fastify) | `21400` | `4855` | `35196` |
| **26%** | [Hono](#hono) | `19297` | `4782` | `29303` |
| **23%** | [Koa](#koa) | `17362` | `9051` | `81698` |
| **11%** | [Carbon](#carbon) | `8288` | `1449` | `10471` |
| **8%** | [Express](#express) | `6393` | `1006` | `8394` |


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
    Reqs/sec      8724.40    5865.18   73050.31
    Latency        5.72ms     4.30ms   371.18ms
    HTTP codes:
      1xx - 0, 2xx - 91645, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8355
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8355
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
    Reqs/sec      6380.17    1062.14    8487.70
    Latency        7.83ms     3.73ms   357.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     21221.93    4090.51   30493.59
    Latency        2.35ms     1.97ms   178.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     18827.79    3747.63   29706.83
    Latency        2.65ms     1.93ms   175.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.25MB/s
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
    Reqs/sec     62924.67    3054.34   65245.03
    Latency      792.53us    82.39us     3.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.94MB/s
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
    Reqs/sec     18068.58    9359.42   82488.85
    Latency        2.76ms     2.37ms   208.41ms
    HTTP codes:
      1xx - 0, 2xx - 90052, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9948
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9948
    Throughput:     3.68MB/s
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
    Reqs/sec     24197.46    6309.37   58993.57
    Latency        2.06ms     1.88ms   162.40ms
    HTTP codes:
      1xx - 0, 2xx - 97601, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2399
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2399
    Throughput:     5.41MB/s
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
    Reqs/sec     78789.73    3530.34   89048.27
    Latency      631.87us   180.84us     9.35ms
    HTTP codes:
      1xx - 0, 2xx - 96736, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3264
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3264
    Throughput:    12.07MB/s
  ```


