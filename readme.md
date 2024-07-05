## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75842` | `6271` | `83324` |
| **84%** | [Hyper Express](#hyper-express) | `63932` | `3743` | `68645` |
| **35%** | [Node (Default)](#node-default) | `26525` | `7964` | `42539` |
| **32%** | [Fastify](#fastify) | `24099` | `7307` | `37001` |
| **27%** | [Hono](#hono) | `20337` | `5617` | `29127` |
| **23%** | [Koa](#koa) | `17653` | `5985` | `49201` |
| **11%** | [Carbon](#carbon) | `8144` | `1431` | `10317` |
| **9%** | [Express](#express) | `6455` | `998` | `8476` |


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
    Reqs/sec      8713.36    4330.69   53904.50
    Latency        5.73ms     4.18ms   364.49ms
    HTTP codes:
      1xx - 0, 2xx - 93753, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6247
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6247
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
    Reqs/sec      6578.08    1054.27    8460.49
    Latency        7.60ms     3.44ms   335.39ms
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
    Reqs/sec     24789.83    7839.83   36574.62
    Latency        2.01ms     2.04ms   183.25ms
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
    Reqs/sec     20342.98    5499.07   29172.07
    Latency        2.46ms     1.99ms   178.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     65571.08    3413.37   76158.33
    Latency      760.74us    71.32us     4.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.31MB/s
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
    Reqs/sec     18297.57    7040.88   64564.39
    Latency        2.72ms     2.44ms   212.41ms
    HTTP codes:
      1xx - 0, 2xx - 93120, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6880
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6880
    Throughput:     3.86MB/s
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
    Reqs/sec     26034.67    8116.81   40971.52
    Latency        1.91ms     1.97ms   164.39ms
    HTTP codes:
      1xx - 0, 2xx - 98265, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1735
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1730
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 5
    Throughput:     5.87MB/s
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
    Reqs/sec     75480.73    6719.92   84003.43
    Latency      660.93us   185.61us    11.46ms
    HTTP codes:
      1xx - 0, 2xx - 97945, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2055
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2055
    Throughput:    11.69MB/s
  ```


