## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74045` | `5502` | `81005` |
| **85%** | [Hyper Express](#hyper-express) | `62708` | `3784` | `70088` |
| **35%** | [Node (Default)](#node-default) | `25886` | `7894` | `69136` |
| **33%** | [Fastify](#fastify) | `24535` | `7298` | `36460` |
| **28%** | [Hono](#hono) | `20393` | `5399` | `31329` |
| **26%** | [Koa](#koa) | `19494` | `7306` | `56797` |
| **11%** | [Carbon](#carbon) | `8269` | `1403` | `10941` |
| **9%** | [Express](#express) | `6636` | `1029` | `8931` |


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
    Reqs/sec      8641.82    3727.98   48027.23
    Latency        5.78ms     4.14ms   359.65ms
    HTTP codes:
      1xx - 0, 2xx - 95005, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4995
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4995
    Throughput:     1.86MB/s
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
    Reqs/sec      7138.87    4009.98   57803.05
    Latency        7.00ms     3.80ms   343.30ms
    HTTP codes:
      1xx - 0, 2xx - 92943, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7057
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7042
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 15
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
    Reqs/sec     23987.67    6784.93   37275.08
    Latency        2.08ms     1.96ms   178.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.44MB/s
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
    Reqs/sec     21260.25    5923.13   31064.14
    Latency        2.35ms     1.92ms   171.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     63731.14    3943.63   67748.76
    Latency      782.46us    72.65us     3.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     18112.40    5633.36   45165.62
    Latency        2.75ms     2.36ms   203.00ms
    HTTP codes:
      1xx - 0, 2xx - 95986, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4014
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4014
    Throughput:     3.94MB/s
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
    Reqs/sec     26392.96    7597.35   45740.88
    Latency        1.89ms     1.85ms   165.75ms
    HTTP codes:
      1xx - 0, 2xx - 98316, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1684
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1684
    Throughput:     5.93MB/s
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
    Reqs/sec     74176.82    5903.07   84848.26
    Latency      671.36us   251.33us    15.21ms
    HTTP codes:
      1xx - 0, 2xx - 97310, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2690
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2690
    Throughput:    11.42MB/s
  ```


