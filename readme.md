## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72376` | `4517` | `78958` |
| **84%** | [Hyper Express](#hyper-express) | `60736` | `2630` | `63456` |
| **36%** | [Node (Default)](#node-default) | `25723` | `7249` | `60240` |
| **33%** | [Fastify](#fastify) | `24069` | `7579` | `37010` |
| **28%** | [Hono](#hono) | `20506` | `5956` | `30373` |
| **26%** | [Koa](#koa) | `18708` | `7420` | `61144` |
| **12%** | [Carbon](#carbon) | `8343` | `1442` | `10373` |
| **9%** | [Express](#express) | `6244` | `969` | `8265` |


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
    Reqs/sec      8957.52    6273.28   67254.88
    Latency        5.56ms     4.32ms   374.57ms
    HTTP codes:
      1xx - 0, 2xx - 89105, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10895
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10895
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
    Reqs/sec      6825.62    4111.67   60247.63
    Latency        7.32ms     3.93ms   360.68ms
    HTTP codes:
      1xx - 0, 2xx - 92946, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7054
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7054
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
    Reqs/sec     23842.40    7288.54   37026.03
    Latency        2.09ms     2.06ms   187.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     20834.39    6156.00   31145.18
    Latency        2.40ms     2.03ms   184.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     60777.53    3459.74   65359.11
    Latency      819.05us    80.19us     3.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.65MB/s
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
    Reqs/sec     18343.94    7064.10   57341.56
    Latency        2.72ms     2.33ms   205.83ms
    HTTP codes:
      1xx - 0, 2xx - 93605, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6395
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6395
    Throughput:     3.88MB/s
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
    Reqs/sec     25534.95    7518.38   58603.18
    Latency        1.96ms     1.89ms   162.78ms
    HTTP codes:
      1xx - 0, 2xx - 97203, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2797
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2797
    Throughput:     5.67MB/s
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
    Reqs/sec     72444.47    3388.05   77433.69
    Latency      687.08us   193.54us    10.71ms
    HTTP codes:
      1xx - 0, 2xx - 96206, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3794
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3794
    Throughput:    11.03MB/s
  ```


