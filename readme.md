## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71250` | `5295` | `86387` |
| **79%** | [Hyper Express](#hyper-express) | `56238` | `4918` | `72073` |
| **30%** | [Hono](#hono) | `21238` | `6432` | `31318` |
| **29%** | [Node (Default)](#node-default) | `20597` | `6245` | `63170` |
| **29%** | [Fastify](#fastify) | `20444` | `5485` | `36591` |
| **25%** | [Koa](#koa) | `17629` | `7711` | `60870` |
| **10%** | [Carbon](#carbon) | `7312` | `1249` | `10270` |
| **9%** | [Express](#express) | `6218` | `1133` | `8336` |


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
    Reqs/sec      8164.64    6665.13   67495.63
    Latency        6.11ms     4.90ms   413.75ms
    HTTP codes:
      1xx - 0, 2xx - 88544, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11456
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11456
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
    Reqs/sec      6187.90    1135.49    8235.55
    Latency        8.08ms     3.99ms   378.54ms
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
    Reqs/sec     19729.88    5280.87   35914.60
    Latency        2.53ms     2.05ms   187.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.48MB/s
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
    Reqs/sec     19975.40    5507.87   29088.32
    Latency        2.50ms     2.15ms   192.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.51MB/s
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
    Reqs/sec     57808.79    3453.63   63294.53
    Latency        0.86ms   103.96us     3.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.21MB/s
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
    Reqs/sec     20398.90   11153.34   76209.66
    Latency        2.45ms     2.65ms   230.33ms
    HTTP codes:
      1xx - 0, 2xx - 87607, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12393
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12393
    Throughput:     4.04MB/s
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
    Reqs/sec     20844.70    7366.65   74351.33
    Latency        2.39ms     2.02ms   177.92ms
    HTTP codes:
      1xx - 0, 2xx - 94298, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5702
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5702
    Throughput:     4.51MB/s
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
    Reqs/sec     68858.91    4462.17   82220.22
    Latency      722.99us   201.09us    11.26ms
    HTTP codes:
      1xx - 0, 2xx - 96644, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3356
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3356
    Throughput:    10.53MB/s
  ```


