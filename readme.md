## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75239` | `2177` | `79667` |
| **84%** | [Hyper Express](#hyper-express) | `63039` | `3810` | `65680` |
| **36%** | [Node (Default)](#node-default) | `26804` | `8101` | `61840` |
| **31%** | [Fastify](#fastify) | `23624` | `7363` | `36400` |
| **28%** | [Hono](#hono) | `20922` | `6210` | `31473` |
| **25%** | [Koa](#koa) | `18729` | `8881` | `77853` |
| **11%** | [Carbon](#carbon) | `8269` | `1423` | `10468` |
| **9%** | [Express](#express) | `6495` | `1059` | `8410` |


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
    Reqs/sec      9071.57    6384.38   79378.14
    Latency        5.50ms     4.26ms   368.94ms
    HTTP codes:
      1xx - 0, 2xx - 91253, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8747
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8747
    Throughput:     1.88MB/s
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
    Reqs/sec      7262.84    6514.07   78438.76
    Latency        6.87ms     3.98ms   364.68ms
    HTTP codes:
      1xx - 0, 2xx - 88809, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11191
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11191
    Throughput:     1.85MB/s
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
    Reqs/sec     24561.74    7390.33   35276.84
    Latency        2.03ms     1.94ms   178.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     20710.33    5980.76   30116.43
    Latency        2.41ms     2.03ms   181.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     62472.00    3468.00   66483.89
    Latency      798.21us    68.05us     2.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     18854.30   10071.02   79633.56
    Latency        2.65ms     2.45ms   212.03ms
    HTTP codes:
      1xx - 0, 2xx - 89369, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10631
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10631
    Throughput:     3.81MB/s
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
    Reqs/sec     26180.98    8735.12   74934.57
    Latency        1.91ms     1.88ms   163.75ms
    HTTP codes:
      1xx - 0, 2xx - 96455, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3545
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3545
    Throughput:     5.78MB/s
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
    Reqs/sec     74523.78    2835.29   80433.71
    Latency      668.15us   214.54us    11.08ms
    HTTP codes:
      1xx - 0, 2xx - 95912, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4088
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4088
    Throughput:    11.30MB/s
  ```


