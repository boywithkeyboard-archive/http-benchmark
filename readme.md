## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73553` | `3009` | `84885` |
| **80%** | [Hyper Express](#hyper-express) | `58858` | `3117` | `61504` |
| **33%** | [Node (Default)](#node-default) | `24241` | `8285` | `68011` |
| **31%** | [Fastify](#fastify) | `22975` | `7481` | `35527` |
| **27%** | [Hono](#hono) | `19631` | `5895` | `30132` |
| **24%** | [Koa](#koa) | `17371` | `7685` | `68539` |
| **11%** | [Carbon](#carbon) | `8024` | `1503` | `10434` |
| **8%** | [Express](#express) | `6021` | `1029` | `8071` |


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
    Reqs/sec      8769.11    6618.16   75822.83
    Latency        5.68ms     4.44ms   382.52ms
    HTTP codes:
      1xx - 0, 2xx - 89758, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10242
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10242
    Throughput:     1.79MB/s
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
    Reqs/sec      6112.47    1056.15    8225.67
    Latency        8.18ms     3.68ms   355.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     22962.34    7624.96   35768.03
    Latency        2.18ms     2.20ms   199.96ms
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
    Reqs/sec     19855.35    6165.73   29319.28
    Latency        2.52ms     2.10ms   187.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.48MB/s
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
    Reqs/sec     59357.36    3244.94   61603.19
    Latency      838.83us    72.94us     3.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.44MB/s
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
    Reqs/sec     17674.02    9373.81   79010.39
    Latency        2.82ms     2.62ms   227.14ms
    HTTP codes:
      1xx - 0, 2xx - 90406, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9594
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9594
    Throughput:     3.61MB/s
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
    Reqs/sec     24213.00    8189.29   62858.93
    Latency        2.06ms     2.09ms   176.94ms
    HTTP codes:
      1xx - 0, 2xx - 97288, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2712
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2712
    Throughput:     5.40MB/s
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
    Reqs/sec     72815.54    2160.77   76566.37
    Latency      683.78us   128.47us     6.70ms
    HTTP codes:
      1xx - 0, 2xx - 97406, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2594
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2594
    Throughput:    11.23MB/s
  ```


