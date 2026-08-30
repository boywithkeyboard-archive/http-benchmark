## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `125017` | `3167` | `132820` |
| **92%** | [Hyper Express](#hyper-express) | `114747` | `6168` | `117799` |
| **58%** | [Node (Default)](#node-default) | `72689` | `16156` | `118161` |
| **58%** | [Fastify](#fastify) | `72406` | `17528` | `85996` |
| **55%** | [Hono](#hono) | `68543` | `17485` | `83017` |
| **52%** | [Koa](#koa) | `65601` | `22092` | `135256` |
| **23%** | [Carbon](#carbon) | `28384` | `8123` | `39023` |
| **15%** | [Express](#express) | `18734` | `3771` | `25123` |


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
    Reqs/sec     30020.17   15417.16  136059.55
    Latency        1.66ms     2.89ms   241.29ms
    HTTP codes:
      1xx - 0, 2xx - 89986, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10014
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10014
    Throughput:     6.13MB/s
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
    Reqs/sec     20505.81   10739.77  111374.46
    Latency        2.43ms     2.06ms   185.02ms
    HTTP codes:
      1xx - 0, 2xx - 92184, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7816
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7816
    Throughput:     5.42MB/s
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
    Reqs/sec     73381.00   17997.35  110820.21
    Latency      679.09us     0.99ms    78.34ms
    HTTP codes:
      1xx - 0, 2xx - 86768, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13232
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13232
    Throughput:    14.44MB/s
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
    Reqs/sec     66651.47   18790.44  117240.31
    Latency      748.51us     0.92ms    68.85ms
    HTTP codes:
      1xx - 0, 2xx - 93120, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6880
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6880
    Throughput:    14.02MB/s
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
    Reqs/sec    114597.66    5237.44  127841.66
    Latency      434.10us   170.62us     8.29ms
    HTTP codes:
      1xx - 0, 2xx - 93438, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6562
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6562
    Throughput:    15.21MB/s
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
    Reqs/sec     62199.50   18935.27  131058.07
    Latency      797.68us     1.18ms    95.34ms
    HTTP codes:
      1xx - 0, 2xx - 91515, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8485
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8485
    Throughput:    12.92MB/s
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
    Reqs/sec     74822.40   17153.96  113539.74
    Latency      665.46us     0.89ms    68.29ms
    HTTP codes:
      1xx - 0, 2xx - 97138, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2862
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2862
    Throughput:    16.67MB/s
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
    Reqs/sec    124570.17    6136.23  131639.38
    Latency      399.82us   106.43us     5.24ms
    HTTP codes:
      1xx - 0, 2xx - 96114, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3886
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3886
    Throughput:    18.93MB/s
  ```


