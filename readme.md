## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74850` | `2086` | `80797` |
| **85%** | [Hyper Express](#hyper-express) | `63608` | `4606` | `75369` |
| **31%** | [Node (Default)](#node-default) | `23210` | `6443` | `61209` |
| **30%** | [Fastify](#fastify) | `22555` | `5841` | `36567` |
| **25%** | [Hono](#hono) | `18626` | `4658` | `30030` |
| **23%** | [Koa](#koa) | `17292` | `8743` | `84972` |
| **11%** | [Carbon](#carbon) | `8106` | `1414` | `10294` |
| **9%** | [Express](#express) | `6545` | `1100` | `8397` |


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
    Reqs/sec      8987.97    5777.75   73379.77
    Latency        5.55ms     4.30ms   372.14ms
    HTTP codes:
      1xx - 0, 2xx - 91986, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8014
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8014
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
    Reqs/sec      6509.32    1099.12    8299.93
    Latency        7.68ms     3.59ms   345.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     21225.55    4724.44   34674.80
    Latency        2.35ms     2.09ms   187.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     18016.07    3795.01   29081.67
    Latency        2.77ms     1.97ms   176.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.07MB/s
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
    Reqs/sec     62588.91    3562.19   64904.34
    Latency      796.57us    72.01us     4.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     16889.24    8280.42   79842.75
    Latency        2.95ms     2.43ms   213.46ms
    HTTP codes:
      1xx - 0, 2xx - 91243, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8757
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8757
    Throughput:     3.48MB/s
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
    Reqs/sec     23484.74    6024.99   59819.13
    Latency        2.13ms     1.96ms   170.25ms
    HTTP codes:
      1xx - 0, 2xx - 97497, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2503
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2503
    Throughput:     5.23MB/s
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
    Reqs/sec     74877.22    2648.37   80432.06
    Latency      664.62us   194.68us    11.30ms
    HTTP codes:
      1xx - 0, 2xx - 96448, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3552
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3552
    Throughput:    11.43MB/s
  ```


