## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73665` | `2809` | `77411` |
| **84%** | [Hyper Express](#hyper-express) | `61839` | `3183` | `65077` |
| **34%** | [Node (Default)](#node-default) | `25137` | `7847` | `68177` |
| **33%** | [Fastify](#fastify) | `24063` | `7612` | `34991` |
| **28%** | [Hono](#hono) | `20647` | `6055` | `29606` |
| **26%** | [Koa](#koa) | `19166` | `8958` | `70072` |
| **11%** | [Carbon](#carbon) | `8006` | `1488` | `10096` |
| **8%** | [Express](#express) | `6103` | `1009` | `8200` |


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
    Reqs/sec      8701.50    6183.34   77273.83
    Latency        5.73ms     4.50ms   387.77ms
    HTTP codes:
      1xx - 0, 2xx - 91243, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8757
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8757
    Throughput:     1.80MB/s
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
    Reqs/sec      6182.52    1032.65    8077.89
    Latency        8.08ms     3.89ms   372.47ms
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
    Reqs/sec     23324.91    6789.53   34694.35
    Latency        2.14ms     2.03ms   182.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.29MB/s
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
    Reqs/sec     19745.71    5281.97   29162.55
    Latency        2.53ms     2.11ms   189.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.46MB/s
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
    Reqs/sec     61324.41    3184.09   63963.40
    Latency      812.82us    74.48us     2.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     20035.78    9125.46   79951.19
    Latency        2.49ms     2.46ms   215.80ms
    HTTP codes:
      1xx - 0, 2xx - 91632, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8368
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8368
    Throughput:     4.15MB/s
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
    Reqs/sec     26194.98    9204.63   73467.99
    Latency        1.91ms     2.02ms   176.20ms
    HTTP codes:
      1xx - 0, 2xx - 96040, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3960
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3960
    Throughput:     5.75MB/s
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
    Reqs/sec     73944.04    3483.71   80757.35
    Latency      673.67us   137.08us     5.52ms
    HTTP codes:
      1xx - 0, 2xx - 97369, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2631
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2631
    Throughput:    11.39MB/s
  ```


