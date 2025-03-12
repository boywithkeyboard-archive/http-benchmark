## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74973` | `5781` | `92317` |
| **85%** | [Hyper Express](#hyper-express) | `63828` | `4187` | `71875` |
| **36%** | [Node (Default)](#node-default) | `27319` | `8461` | `62397` |
| **34%** | [Fastify](#fastify) | `25196` | `7994` | `38034` |
| **29%** | [Hono](#hono) | `21820` | `5848` | `31263` |
| **27%** | [Koa](#koa) | `19877` | `7327` | `56776` |
| **11%** | [Carbon](#carbon) | `8479` | `1437` | `10486` |
| **9%** | [Express](#express) | `6645` | `1114` | `8529` |


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
    Reqs/sec      8980.47    5118.80   61342.63
    Latency        5.56ms     4.17ms   365.45ms
    HTTP codes:
      1xx - 0, 2xx - 92612, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7388
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7388
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
    Reqs/sec      7073.60    4611.04   54318.91
    Latency        7.05ms     3.90ms   354.36ms
    HTTP codes:
      1xx - 0, 2xx - 91575, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8425
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8425
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
    Reqs/sec     24109.92    7108.42   36150.03
    Latency        2.07ms     2.15ms   191.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     20633.99    5621.92   30018.14
    Latency        2.42ms     1.94ms   176.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     62502.22    3806.35   65558.68
    Latency      797.80us    72.92us     3.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     18540.07    6224.51   48731.58
    Latency        2.69ms     2.34ms   204.68ms
    HTTP codes:
      1xx - 0, 2xx - 94495, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5505
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5505
    Throughput:     3.96MB/s
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
    Reqs/sec     26915.53    8349.82   57346.20
    Latency        1.85ms     1.91ms   160.75ms
    HTTP codes:
      1xx - 0, 2xx - 96710, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3290
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3290
    Throughput:     5.96MB/s
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
    Reqs/sec     74610.94    5750.40   85459.34
    Latency      668.83us   172.00us     8.37ms
    HTTP codes:
      1xx - 0, 2xx - 96965, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3035
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3035
    Throughput:    11.43MB/s
  ```


