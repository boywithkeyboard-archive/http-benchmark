## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68737` | `4771` | `78271` |
| **82%** | [Hyper Express](#hyper-express) | `56635` | `3259` | `62976` |
| **32%** | [Hono](#hono) | `21764` | `6935` | `30309` |
| **30%** | [Fastify](#fastify) | `20328` | `6062` | `36297` |
| **29%** | [Node (Default)](#node-default) | `20073` | `5590` | `63533` |
| **27%** | [Koa](#koa) | `18288` | `8458` | `75684` |
| **11%** | [Carbon](#carbon) | `7340` | `1264` | `10298` |
| **9%** | [Express](#express) | `6022` | `1109` | `8140` |


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
    Reqs/sec      7589.75    5094.81   63609.31
    Latency        6.57ms     4.83ms   404.93ms
    HTTP codes:
      1xx - 0, 2xx - 92098, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7902
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7902
    Throughput:     1.59MB/s
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
    Reqs/sec      6001.43    1100.98    8054.78
    Latency        8.33ms     4.05ms   383.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.72MB/s
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
    Reqs/sec     20101.62    4669.10   35271.58
    Latency        2.49ms     2.20ms   195.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     21957.76    7183.95   30440.66
    Latency        2.27ms     2.34ms   201.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.96MB/s
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
    Reqs/sec     57991.46    3182.73   65441.08
    Latency        0.86ms   103.96us     4.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.24MB/s
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
    Reqs/sec     19793.37    8689.14   64161.24
    Latency        2.52ms     2.55ms   221.29ms
    HTTP codes:
      1xx - 0, 2xx - 92592, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7408
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7408
    Throughput:     4.14MB/s
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
    Reqs/sec     19330.69    4394.21   55315.05
    Latency        2.58ms     2.11ms   179.12ms
    HTTP codes:
      1xx - 0, 2xx - 97649, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2351
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2351
    Throughput:     4.32MB/s
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
    Reqs/sec     69578.19    4000.94   80521.51
    Latency      715.22us   170.36us     8.21ms
    HTTP codes:
      1xx - 0, 2xx - 95400, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4600
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4600
    Throughput:    10.51MB/s
  ```


