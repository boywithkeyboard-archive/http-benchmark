## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74242` | `5980` | `81692` |
| **82%** | [Hyper Express](#hyper-express) | `60738` | `5012` | `78228` |
| **35%** | [Node (Default)](#node-default) | `25851` | `7303` | `39253` |
| **34%** | [Fastify](#fastify) | `25263` | `7591` | `36634` |
| **28%** | [Hono](#hono) | `20867` | `5646` | `30138` |
| **25%** | [Koa](#koa) | `18338` | `5840` | `43368` |
| **11%** | [Carbon](#carbon) | `8412` | `1395` | `10712` |
| **9%** | [Express](#express) | `6659` | `1048` | `8660` |


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
    Reqs/sec      8749.13    3789.32   56823.56
    Latency        5.70ms     4.13ms   356.37ms
    HTTP codes:
      1xx - 0, 2xx - 94897, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5103
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5103
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
    Reqs/sec      6673.28    1061.79    8776.42
    Latency        7.49ms     3.46ms   331.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.91MB/s
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
    Reqs/sec     24312.53    7031.68   35948.11
    Latency        2.06ms     1.96ms   174.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     20789.00    5605.97   29880.79
    Latency        2.40ms     1.96ms   177.25ms
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
    Reqs/sec     61802.51    3766.63   66494.36
    Latency      806.55us    70.73us     2.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.78MB/s
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
    Reqs/sec     17788.99    5827.84   47306.11
    Latency        2.80ms     2.39ms   208.39ms
    HTTP codes:
      1xx - 0, 2xx - 95465, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4535
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4535
    Throughput:     3.84MB/s
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
    Reqs/sec     26398.84    7944.39   47423.23
    Latency        1.89ms     1.85ms   159.51ms
    HTTP codes:
      1xx - 0, 2xx - 97633, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2367
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2367
    Throughput:     5.90MB/s
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
    Reqs/sec     74271.40    5318.05   82931.35
    Latency      670.84us   158.03us     9.74ms
    HTTP codes:
      1xx - 0, 2xx - 97904, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2096
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2096
    Throughput:    11.51MB/s
  ```


