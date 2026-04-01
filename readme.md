## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71514` | `3804` | `85739` |
| **84%** | [Hyper Express](#hyper-express) | `59951` | `3987` | `68647` |
| **32%** | [Fastify](#fastify) | `22557` | `7263` | `36788` |
| **28%** | [Hono](#hono) | `20045` | `6483` | `31052` |
| **28%** | [Node (Default)](#node-default) | `19721` | `5503` | `75124` |
| **26%** | [Koa](#koa) | `18456` | `8708` | `73228` |
| **10%** | [Carbon](#carbon) | `7361` | `1136` | `10472` |
| **9%** | [Express](#express) | `6143` | `1168` | `8417` |


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
    Reqs/sec      7891.63    5706.57   75670.55
    Latency        6.32ms     4.63ms   396.60ms
    HTTP codes:
      1xx - 0, 2xx - 91366, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8634
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8634
    Throughput:     1.64MB/s
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
    Reqs/sec      6251.72    1081.60    8398.53
    Latency        7.99ms     3.81ms   368.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     23591.91    8474.51   37035.11
    Latency        2.12ms     2.04ms   185.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.35MB/s
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
    Reqs/sec     21620.45    6598.90   30140.05
    Latency        2.31ms     2.28ms   202.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.89MB/s
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
    Reqs/sec     59800.71    4144.65   69564.91
    Latency      834.02us    94.64us     3.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.49MB/s
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
    Reqs/sec     19313.38    8567.09   64509.60
    Latency        2.58ms     2.51ms   217.55ms
    HTTP codes:
      1xx - 0, 2xx - 92910, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7090
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7090
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
    Reqs/sec     20940.90    5540.52   67788.27
    Latency        2.38ms     2.00ms   171.09ms
    HTTP codes:
      1xx - 0, 2xx - 96523, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3477
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3477
    Throughput:     4.63MB/s
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
    Reqs/sec     72149.94    4184.81   84646.28
    Latency      690.61us   194.71us     8.71ms
    HTTP codes:
      1xx - 0, 2xx - 96973, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3027
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3027
    Throughput:    11.06MB/s
  ```


