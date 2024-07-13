## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73482` | `5955` | `83055` |
| **84%** | [Hyper Express](#hyper-express) | `62072` | `3802` | `67314` |
| **35%** | [Node (Default)](#node-default) | `25677` | `8006` | `47023` |
| **33%** | [Fastify](#fastify) | `24473` | `7546` | `36473` |
| **28%** | [Hono](#hono) | `20553` | `5877` | `30472` |
| **25%** | [Koa](#koa) | `18048` | `6477` | `55721` |
| **11%** | [Carbon](#carbon) | `8036` | `1356` | `10346` |
| **9%** | [Express](#express) | `6420` | `1000` | `8411` |


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
    Reqs/sec      8195.44    3255.79   49558.72
    Latency        6.08ms     4.35ms   377.34ms
    HTTP codes:
      1xx - 0, 2xx - 95586, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4414
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4414
    Throughput:     1.78MB/s
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
    Reqs/sec      6351.25    1027.44   12497.13
    Latency        7.88ms     3.68ms   352.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     22929.93    7064.80   35957.46
    Latency        2.18ms     2.04ms   184.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.20MB/s
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
    Reqs/sec     20424.92    5413.06   30266.25
    Latency        2.45ms     2.04ms   183.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     63044.96    4015.37   72283.47
    Latency      790.01us    75.75us     3.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.96MB/s
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
    Reqs/sec     17662.92    6490.80   57446.62
    Latency        2.82ms     2.33ms   204.13ms
    HTTP codes:
      1xx - 0, 2xx - 93377, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6623
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6623
    Throughput:     3.73MB/s
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
    Reqs/sec     25280.96    7109.87   41446.49
    Latency        1.97ms     1.89ms   161.59ms
    HTTP codes:
      1xx - 0, 2xx - 98381, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1619
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1619
    Throughput:     5.70MB/s
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
    Reqs/sec     75455.08    6804.62   94979.41
    Latency      659.67us   234.17us    11.26ms
    HTTP codes:
      1xx - 0, 2xx - 96993, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3007
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3007
    Throughput:    11.57MB/s
  ```


