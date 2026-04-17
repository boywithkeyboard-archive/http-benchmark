## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72066` | `4725` | `85282` |
| **81%** | [Hyper Express](#hyper-express) | `58700` | `5064` | `67959` |
| **29%** | [Fastify](#fastify) | `20874` | `6606` | `36803` |
| **29%** | [Hono](#hono) | `20622` | `5729` | `30000` |
| **28%** | [Node (Default)](#node-default) | `20015` | `4818` | `57368` |
| **24%** | [Koa](#koa) | `17338` | `7708` | `64704` |
| **10%** | [Carbon](#carbon) | `7427` | `1259` | `10281` |
| **9%** | [Express](#express) | `6275` | `1159` | `8254` |


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
    Reqs/sec      7751.21    5284.99   69469.42
    Latency        6.44ms     4.50ms   382.77ms
    HTTP codes:
      1xx - 0, 2xx - 92550, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7450
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7450
    Throughput:     1.63MB/s
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
    Reqs/sec      6164.40    1116.94    8539.49
    Latency        8.11ms     3.93ms   376.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     20535.09    5342.77   36054.27
    Latency        2.43ms     2.05ms   183.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     22336.51    6991.31   31083.02
    Latency        2.24ms     2.06ms   181.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.04MB/s
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
    Reqs/sec     59659.52    3364.92   68270.48
    Latency      835.72us    86.61us     3.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.48MB/s
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
    Reqs/sec     17999.50    8749.67   74128.12
    Latency        2.77ms     2.49ms   218.48ms
    HTTP codes:
      1xx - 0, 2xx - 92815, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7185
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7185
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
    Reqs/sec     20616.71    5243.51   60698.87
    Latency        2.42ms     2.00ms   170.34ms
    HTTP codes:
      1xx - 0, 2xx - 97082, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2918
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2918
    Throughput:     4.59MB/s
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
    Reqs/sec     73241.09    5158.64   89101.15
    Latency      678.78us   184.66us     9.80ms
    HTTP codes:
      1xx - 0, 2xx - 93969, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6031
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6031
    Throughput:    10.90MB/s
  ```


