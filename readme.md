## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75075` | `6766` | `88531` |
| **83%** | [Hyper Express](#hyper-express) | `62678` | `3935` | `68630` |
| **33%** | [Node (Default)](#node-default) | `24481` | `6555` | `47875` |
| **32%** | [Fastify](#fastify) | `24113` | `7563` | `35543` |
| **28%** | [Hono](#hono) | `20704` | `6009` | `29333` |
| **24%** | [Koa](#koa) | `17710` | `6531` | `58003` |
| **11%** | [Carbon](#carbon) | `8171` | `1412` | `10414` |
| **9%** | [Express](#express) | `6558` | `1055` | `8464` |


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
    Reqs/sec      8766.13    4734.72   64589.99
    Latency        5.69ms     4.27ms   370.76ms
    HTTP codes:
      1xx - 0, 2xx - 93067, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6933
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6933
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
    Reqs/sec      6511.69    1016.17    8477.93
    Latency        7.68ms     3.54ms   338.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24544.30    7355.75   35140.48
    Latency        2.04ms     1.97ms   177.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     20182.38    5320.10   28984.13
    Latency        2.48ms     2.00ms   179.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     62853.49    4164.19   74722.69
    Latency      793.04us    69.95us     4.52ms
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
    Reqs/sec     17738.14    6268.48   55308.43
    Latency        2.81ms     2.31ms   201.91ms
    HTTP codes:
      1xx - 0, 2xx - 94182, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5818
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5818
    Throughput:     3.78MB/s
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
    Reqs/sec     26162.91    7676.35   56477.61
    Latency        1.91ms     1.85ms   159.25ms
    HTTP codes:
      1xx - 0, 2xx - 97441, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2559
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2559
    Throughput:     5.84MB/s
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
    Reqs/sec     73013.45    5409.12   79917.29
    Latency      682.80us   182.54us     9.87ms
    HTTP codes:
      1xx - 0, 2xx - 97306, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2694
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2694
    Throughput:    11.24MB/s
  ```


