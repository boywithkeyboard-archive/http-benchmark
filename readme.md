## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75222` | `5074` | `91192` |
| **83%** | [Hyper Express](#hyper-express) | `62597` | `3841` | `71049` |
| **37%** | [Node (Default)](#node-default) | `27677` | `8596` | `54475` |
| **33%** | [Fastify](#fastify) | `24543` | `7470` | `35951` |
| **28%** | [Hono](#hono) | `21393` | `5270` | `29912` |
| **25%** | [Koa](#koa) | `18550` | `7173` | `57705` |
| **11%** | [Carbon](#carbon) | `8472` | `1465` | `10379` |
| **8%** | [Express](#express) | `6363` | `966` | `8322` |


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
    Reqs/sec      8941.55    4629.00   57288.14
    Latency        5.58ms     4.33ms   370.33ms
    HTTP codes:
      1xx - 0, 2xx - 93389, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6611
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6611
    Throughput:     1.90MB/s
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
    Reqs/sec      7004.83    4646.05   58462.84
    Latency        7.13ms     3.95ms   358.63ms
    HTTP codes:
      1xx - 0, 2xx - 91638, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8362
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8362
    Throughput:     1.84MB/s
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
    Reqs/sec     24769.17    7427.25   36264.83
    Latency        2.02ms     2.03ms   181.09ms
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
    Reqs/sec     22513.56    6135.57   30781.45
    Latency        2.22ms     1.86ms   164.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.09MB/s
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
    Reqs/sec     61936.78    3793.30   65100.81
    Latency      805.21us    77.81us     3.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     18671.48    6533.25   54245.63
    Latency        2.67ms     2.32ms   202.68ms
    HTTP codes:
      1xx - 0, 2xx - 94010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5990
    Throughput:     3.97MB/s
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
    Reqs/sec     27914.35    8354.51   58099.33
    Latency        1.79ms     1.86ms   160.50ms
    HTTP codes:
      1xx - 0, 2xx - 97344, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2656
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2656
    Throughput:     6.22MB/s
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
    Reqs/sec     74146.80    5371.97   83500.64
    Latency      671.45us   145.93us     8.63ms
    HTTP codes:
      1xx - 0, 2xx - 97946, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2054
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2054
    Throughput:    11.49MB/s
  ```


