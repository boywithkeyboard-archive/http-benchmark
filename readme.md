## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75434` | `6032` | `88267` |
| **83%** | [Hyper Express](#hyper-express) | `62719` | `4068` | `73399` |
| **35%** | [Node (Default)](#node-default) | `26397` | `7561` | `48654` |
| **32%** | [Fastify](#fastify) | `24326` | `7093` | `36292` |
| **27%** | [Hono](#hono) | `20438` | `5927` | `29784` |
| **25%** | [Koa](#koa) | `18536` | `6458` | `47035` |
| **11%** | [Carbon](#carbon) | `8364` | `1424` | `10578` |
| **9%** | [Express](#express) | `6644` | `1059` | `8655` |


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
    Reqs/sec      8835.47    4062.14   60513.56
    Latency        5.65ms     4.10ms   357.41ms
    HTTP codes:
      1xx - 0, 2xx - 94396, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5604
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5604
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
    Reqs/sec      7195.48    4258.53   49405.60
    Latency        6.93ms     3.83ms   348.71ms
    HTTP codes:
      1xx - 0, 2xx - 92439, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7561
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7554
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 7
    Throughput:     1.90MB/s
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
    Reqs/sec     25158.46    7430.63   36481.78
    Latency        1.99ms     1.91ms   171.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.70MB/s
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
    Reqs/sec     21022.22    5914.67   30046.21
    Latency        2.38ms     1.91ms   173.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     63789.23    2668.85   65998.07
    Latency      780.27us    58.94us     2.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.07MB/s
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
    Reqs/sec     18301.93    6589.85   57843.50
    Latency        2.72ms     2.32ms   200.36ms
    HTTP codes:
      1xx - 0, 2xx - 94598, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5402
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5402
    Throughput:     3.92MB/s
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
    Reqs/sec     26891.06    8408.94   58512.22
    Latency        1.86ms     1.76ms   150.90ms
    HTTP codes:
      1xx - 0, 2xx - 96694, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3306
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3306
    Throughput:     5.94MB/s
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
    Reqs/sec     74304.92    6287.90   79450.04
    Latency      670.72us   160.05us     7.11ms
    HTTP codes:
      1xx - 0, 2xx - 97224, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2776
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2776
    Throughput:    11.42MB/s
  ```


