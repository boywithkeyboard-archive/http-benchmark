## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74404` | `4914` | `93337` |
| **84%** | [Hyper Express](#hyper-express) | `62157` | `3150` | `68811` |
| **30%** | [Hono](#hono) | `22256` | `6422` | `30556` |
| **29%** | [Node (Default)](#node-default) | `21933` | `6327` | `67194` |
| **28%** | [Fastify](#fastify) | `21151` | `5436` | `37292` |
| **25%** | [Koa](#koa) | `18465` | `8867` | `76796` |
| **10%** | [Carbon](#carbon) | `7377` | `1094` | `10082` |
| **8%** | [Express](#express) | `6106` | `969` | `8163` |


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
    Reqs/sec      8221.27    6476.99   75274.72
    Latency        6.07ms     4.32ms   372.20ms
    HTTP codes:
      1xx - 0, 2xx - 89866, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10134
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10134
    Throughput:     1.68MB/s
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
    Reqs/sec      6183.74     991.33    8185.15
    Latency        8.08ms     3.86ms   367.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     20812.38    5287.92   36072.50
    Latency        2.40ms     1.92ms   175.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     23049.09    6912.77   30703.18
    Latency        2.17ms     2.03ms   183.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.21MB/s
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
    Reqs/sec     61401.03    3713.59   71109.05
    Latency      812.59us    81.76us     2.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.72MB/s
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
    Reqs/sec     19213.77    8652.10   69245.23
    Latency        2.60ms     2.51ms   218.79ms
    HTTP codes:
      1xx - 0, 2xx - 92110, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7890
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7890
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
    Reqs/sec     22136.67    6583.60   63776.96
    Latency        2.26ms     1.93ms   165.06ms
    HTTP codes:
      1xx - 0, 2xx - 96048, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3952
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3952
    Throughput:     4.86MB/s
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
    Reqs/sec     73654.08    5108.93   92387.06
    Latency      675.41us   256.99us    15.52ms
    HTTP codes:
      1xx - 0, 2xx - 95852, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4148
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4148
    Throughput:    11.18MB/s
  ```


