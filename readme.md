## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73367` | `4310` | `90983` |
| **84%** | [Hyper Express](#hyper-express) | `61817` | `4332` | `70692` |
| **30%** | [Node (Default)](#node-default) | `21815` | `6346` | `72564` |
| **29%** | [Fastify](#fastify) | `21306` | `6262` | `37005` |
| **28%** | [Hono](#hono) | `20727` | `5998` | `31300` |
| **27%** | [Koa](#koa) | `19704` | `8971` | `80017` |
| **10%** | [Carbon](#carbon) | `7453` | `1194` | `10287` |
| **9%** | [Express](#express) | `6409` | `1093` | `8338` |


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
    Reqs/sec      8311.66    5901.14   67084.71
    Latency        6.00ms     4.37ms   376.56ms
    HTTP codes:
      1xx - 0, 2xx - 90680, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9320
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9320
    Throughput:     1.71MB/s
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
    Reqs/sec      6298.96    1048.56    8355.43
    Latency        7.93ms     3.75ms   360.91ms
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
    Reqs/sec     22513.05    6201.76   38338.09
    Latency        2.22ms     2.11ms   187.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.11MB/s
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
    Reqs/sec     23029.66    6399.15   31500.71
    Latency        2.17ms     1.93ms   172.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.20MB/s
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
    Reqs/sec     61044.16    3070.34   68529.53
    Latency      816.93us    79.32us     2.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.67MB/s
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
    Reqs/sec     18566.92    8461.82   71560.93
    Latency        2.69ms     2.42ms   212.80ms
    HTTP codes:
      1xx - 0, 2xx - 91760, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8240
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8240
    Throughput:     3.85MB/s
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
    Reqs/sec     22268.22    6464.54   67642.06
    Latency        2.24ms     2.01ms   169.28ms
    HTTP codes:
      1xx - 0, 2xx - 96817, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3183
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3183
    Throughput:     4.94MB/s
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
    Reqs/sec     71695.18    4003.99   83786.01
    Latency      694.51us   199.43us     9.88ms
    HTTP codes:
      1xx - 0, 2xx - 96309, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3691
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3691
    Throughput:    10.92MB/s
  ```


