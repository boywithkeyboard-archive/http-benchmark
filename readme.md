## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69206` | `4168` | `80616` |
| **85%** | [Hyper Express](#hyper-express) | `58571` | `3353` | `71182` |
| **30%** | [Fastify](#fastify) | `20607` | `5502` | `36583` |
| **30%** | [Hono](#hono) | `20511` | `5977` | `30494` |
| **29%** | [Node (Default)](#node-default) | `19924` | `4759` | `57581` |
| **26%** | [Koa](#koa) | `18214` | `8148` | `62509` |
| **11%** | [Carbon](#carbon) | `7456` | `1213` | `10333` |
| **9%** | [Express](#express) | `6179` | `1066` | `8283` |


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
    Reqs/sec      8221.78    6256.06   69705.53
    Latency        6.07ms     4.61ms   398.91ms
    HTTP codes:
      1xx - 0, 2xx - 89642, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10358
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10358
    Throughput:     1.67MB/s
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
    Reqs/sec      6188.09    1080.84    8245.70
    Latency        8.08ms     3.85ms   370.44ms
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
    Reqs/sec     20418.43    4755.55   35353.02
    Latency        2.45ms     1.99ms   181.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     20838.95    6374.05   30141.30
    Latency        2.40ms     2.27ms   197.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     58281.11    3264.22   64331.38
    Latency        0.86ms    92.80us     3.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.28MB/s
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
    Reqs/sec     18471.82    8320.06   69991.27
    Latency        2.70ms     2.44ms   213.26ms
    HTTP codes:
      1xx - 0, 2xx - 91885, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8115
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8115
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
    Reqs/sec     20153.43    4963.72   63044.94
    Latency        2.47ms     2.01ms   171.82ms
    HTTP codes:
      1xx - 0, 2xx - 96781, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3219
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3219
    Throughput:     4.47MB/s
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
    Reqs/sec     69949.10    3583.03   81187.71
    Latency      712.46us   129.93us     6.61ms
    HTTP codes:
      1xx - 0, 2xx - 97342, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2658
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2658
    Throughput:    10.77MB/s
  ```


