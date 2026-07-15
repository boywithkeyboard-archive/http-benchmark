## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70488` | `4267` | `80883` |
| **84%** | [Hyper Express](#hyper-express) | `59280` | `3615` | `65511` |
| **31%** | [Hono](#hono) | `22094` | `6950` | `30256` |
| **29%** | [Fastify](#fastify) | `20541` | `5566` | `37314` |
| **29%** | [Node (Default)](#node-default) | `20358` | `4970` | `63146` |
| **27%** | [Koa](#koa) | `18722` | `8254` | `62169` |
| **11%** | [Carbon](#carbon) | `7508` | `1237` | `10407` |
| **9%** | [Express](#express) | `6355` | `1139` | `8456` |


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
    Reqs/sec      8581.38    7895.76   77326.36
    Latency        5.81ms     4.55ms   385.79ms
    HTTP codes:
      1xx - 0, 2xx - 85936, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14064
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14064
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
    Reqs/sec      6233.92    1081.41    8389.58
    Latency        8.02ms     3.82ms   362.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     21021.69    5010.78   37150.08
    Latency        2.38ms     2.12ms   190.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     21585.71    6769.35   31124.25
    Latency        2.32ms     2.13ms   190.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.87MB/s
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
    Reqs/sec     59026.13    3735.76   67016.02
    Latency      844.82us    98.96us     3.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.38MB/s
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
    Reqs/sec     19241.37    8969.45   74750.44
    Latency        2.59ms     2.38ms   212.00ms
    HTTP codes:
      1xx - 0, 2xx - 91886, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8114
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8114
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
    Reqs/sec     20829.22    6471.62   76121.50
    Latency        2.39ms     1.93ms   168.17ms
    HTTP codes:
      1xx - 0, 2xx - 95450, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4550
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4550
    Throughput:     4.56MB/s
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
    Reqs/sec     71126.45    4967.08   88008.13
    Latency      699.57us   155.35us     6.80ms
    HTTP codes:
      1xx - 0, 2xx - 95147, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4853
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4853
    Throughput:    10.72MB/s
  ```


