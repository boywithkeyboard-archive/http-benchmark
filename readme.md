## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `127421` | `6276` | `139576` |
| **81%** | [Hyper Express](#hyper-express) | `103841` | `7151` | `107776` |
| **31%** | [Node (Default)](#node-default) | `39706` | `14434` | `120671` |
| **30%** | [Fastify](#fastify) | `38158` | `8922` | `56863` |
| **26%** | [Hono](#hono) | `33196` | `7303` | `48949` |
| **26%** | [Koa](#koa) | `32848` | `16565` | `123474` |
| **10%** | [Carbon](#carbon) | `13027` | `2709` | `17094` |
| **7%** | [Express](#express) | `9296` | `1768` | `12225` |


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
    Reqs/sec     14353.52   11984.64  130531.33
    Latency        3.47ms     3.66ms   310.14ms
    HTTP codes:
      1xx - 0, 2xx - 87987, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12013
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12013
    Throughput:     2.87MB/s
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
    Reqs/sec     10348.11   10568.77  111237.72
    Latency        4.82ms     3.35ms   305.27ms
    HTTP codes:
      1xx - 0, 2xx - 86542, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13458
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13458
    Throughput:     2.57MB/s
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
    Reqs/sec     38287.07    9389.26   56725.99
    Latency        1.31ms     1.46ms   127.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.68MB/s
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
    Reqs/sec     33580.88    7908.68   50295.68
    Latency        1.49ms     1.39ms   121.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.59MB/s
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
    Reqs/sec    102346.07    6802.69  109843.25
    Latency      486.70us    39.95us     2.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.53MB/s
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
    Reqs/sec     32854.58   14450.00  107920.74
    Latency        1.52ms     1.72ms   149.22ms
    HTTP codes:
      1xx - 0, 2xx - 90140, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9860
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9860
    Throughput:     6.69MB/s
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
    Reqs/sec     41828.60   13041.60  112105.11
    Latency        1.19ms     1.40ms   117.06ms
    HTTP codes:
      1xx - 0, 2xx - 95236, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4764
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4764
    Throughput:     9.13MB/s
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
    Reqs/sec    126577.82    4544.55  130533.08
    Latency      392.42us   153.05us    10.46ms
    HTTP codes:
      1xx - 0, 2xx - 95595, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4405
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4405
    Throughput:    19.15MB/s
  ```


