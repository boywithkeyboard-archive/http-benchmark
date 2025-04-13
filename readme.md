## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75605` | `5560` | `87459` |
| **83%** | [Hyper Express](#hyper-express) | `62885` | `3784` | `65811` |
| **35%** | [Node (Default)](#node-default) | `26553` | `7278` | `55592` |
| **34%** | [Fastify](#fastify) | `25707` | `7679` | `36739` |
| **28%** | [Hono](#hono) | `20958` | `5872` | `30850` |
| **25%** | [Koa](#koa) | `19054` | `7410` | `66887` |
| **11%** | [Carbon](#carbon) | `8400` | `1427` | `10560` |
| **9%** | [Express](#express) | `6674` | `1093` | `8543` |


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
    Reqs/sec      8933.64    4621.30   54452.36
    Latency        5.59ms     4.26ms   368.04ms
    HTTP codes:
      1xx - 0, 2xx - 93023, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6977
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6977
    Throughput:     1.89MB/s
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
    Reqs/sec      7102.90    4427.89   53607.60
    Latency        7.02ms     3.92ms   357.92ms
    HTTP codes:
      1xx - 0, 2xx - 92116, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7884
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7884
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
    Reqs/sec     26237.32    8322.63   38005.01
    Latency        1.90ms     1.93ms   176.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.96MB/s
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
    Reqs/sec     21559.40    6206.62   30502.88
    Latency        2.32ms     2.02ms   181.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     63642.96    3635.72   65970.76
    Latency      783.77us    68.70us     2.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.04MB/s
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
    Reqs/sec     19374.25    8260.89   63653.33
    Latency        2.57ms     2.40ms   208.68ms
    HTTP codes:
      1xx - 0, 2xx - 89591, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10409
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10409
    Throughput:     3.93MB/s
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
    Reqs/sec     26660.50    7122.85   44908.81
    Latency        1.87ms     1.87ms   160.76ms
    HTTP codes:
      1xx - 0, 2xx - 98261, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1739
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1739
    Throughput:     5.99MB/s
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
    Reqs/sec     74894.28    4842.93   84950.72
    Latency      665.06us   197.43us     8.20ms
    HTTP codes:
      1xx - 0, 2xx - 97140, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2860
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2860
    Throughput:    11.51MB/s
  ```


