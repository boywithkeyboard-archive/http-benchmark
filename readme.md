## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74404` | `5532` | `83527` |
| **85%** | [Hyper Express](#hyper-express) | `63132` | `4094` | `73601` |
| **34%** | [Node (Default)](#node-default) | `25332` | `7151` | `50735` |
| **33%** | [Fastify](#fastify) | `24422` | `7327` | `36371` |
| **28%** | [Hono](#hono) | `21063` | `5541` | `29452` |
| **24%** | [Koa](#koa) | `17698` | `5962` | `57446` |
| **11%** | [Carbon](#carbon) | `8323` | `1435` | `10558` |
| **9%** | [Express](#express) | `6573` | `1022` | `8641` |


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
    Reqs/sec      8777.53    4643.54   64771.22
    Latency        5.69ms     4.14ms   359.61ms
    HTTP codes:
      1xx - 0, 2xx - 93517, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6483
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6483
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
    Reqs/sec      6581.35    1037.63    8561.18
    Latency        7.59ms     3.41ms   329.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24704.06    7262.98   35594.87
    Latency        2.02ms     1.90ms   171.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.60MB/s
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
    Reqs/sec     20867.48    5773.49   30681.37
    Latency        2.39ms     1.90ms   175.25ms
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
    Reqs/sec     62899.18    3150.27   76185.48
    Latency      792.65us    71.24us     4.32ms
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
    Reqs/sec     17493.91    5894.92   56090.06
    Latency        2.82ms     2.05ms   185.72ms
    HTTP codes:
      1xx - 0, 2xx - 93966, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6034
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6034
    Throughput:     3.75MB/s
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
    Reqs/sec     25985.20    8315.03   55334.38
    Latency        1.91ms     1.93ms   165.52ms
    HTTP codes:
      1xx - 0, 2xx - 96325, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3675
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3675
    Throughput:     5.76MB/s
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
    Reqs/sec     72266.84    5530.88   79801.43
    Latency      682.78us   178.05us    19.51ms
    HTTP codes:
      1xx - 0, 2xx - 98044, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1956
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1956
    Throughput:    11.24MB/s
  ```


