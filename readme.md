## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74927` | `5872` | `95051` |
| **84%** | [Hyper Express](#hyper-express) | `62594` | `3962` | `68566` |
| **35%** | [Node (Default)](#node-default) | `26241` | `7658` | `38893` |
| **31%** | [Fastify](#fastify) | `23315` | `7067` | `35943` |
| **28%** | [Hono](#hono) | `20776` | `5889` | `30726` |
| **24%** | [Koa](#koa) | `18094` | `6672` | `59015` |
| **11%** | [Carbon](#carbon) | `8296` | `1408` | `10480` |
| **9%** | [Express](#express) | `6442` | `1047` | `8448` |


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
    Reqs/sec      8641.90    2946.00   39272.07
    Latency        5.77ms     4.18ms   362.77ms
    HTTP codes:
      1xx - 0, 2xx - 95916, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4084
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4084
    Throughput:     1.88MB/s
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
    Reqs/sec      6550.72    1055.86    8540.62
    Latency        7.63ms     3.59ms   343.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24341.77    7402.86   35980.70
    Latency        2.05ms     1.89ms   170.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     20892.90    5758.92   29622.18
    Latency        2.39ms     1.99ms   178.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     62561.67    3911.64   65548.96
    Latency      796.98us    77.62us     3.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     18259.33    7054.33   52621.72
    Latency        2.73ms     2.37ms   203.37ms
    HTTP codes:
      1xx - 0, 2xx - 91889, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8111
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8111
    Throughput:     3.79MB/s
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
    Reqs/sec     25508.38    7023.82   45320.59
    Latency        1.95ms     1.92ms   173.53ms
    HTTP codes:
      1xx - 0, 2xx - 97842, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2158
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2158
    Throughput:     5.72MB/s
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
    Reqs/sec     74711.87    5743.22   82316.65
    Latency      665.94us   176.25us    12.91ms
    HTTP codes:
      1xx - 0, 2xx - 97923, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2077
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2077
    Throughput:    11.58MB/s
  ```


