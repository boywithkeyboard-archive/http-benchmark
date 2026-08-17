## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68051` | `4196` | `82948` |
| **82%** | [Hyper Express](#hyper-express) | `56038` | `4753` | `67246` |
| **31%** | [Hono](#hono) | `21019` | `6686` | `30095` |
| **29%** | [Node (Default)](#node-default) | `19783` | `5748` | `77345` |
| **28%** | [Fastify](#fastify) | `19268` | `5317` | `35417` |
| **27%** | [Koa](#koa) | `18574` | `8614` | `77507` |
| **11%** | [Carbon](#carbon) | `7394` | `1331` | `10192` |
| **9%** | [Express](#express) | `6089` | `1112` | `8130` |


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
    Reqs/sec      8171.64    6188.54   64176.24
    Latency        6.10ms     4.71ms   398.30ms
    HTTP codes:
      1xx - 0, 2xx - 90101, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9899
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9899
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
    Reqs/sec      5989.42    1015.61    8172.79
    Latency        8.34ms     3.67ms   354.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.71MB/s
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
    Reqs/sec     20590.71    5452.48   36460.84
    Latency        2.43ms     2.12ms   192.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
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
    Reqs/sec     20363.39    6198.28   29437.15
    Latency        2.45ms     2.16ms   194.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     58268.03    3332.74   64372.07
    Latency        0.86ms    93.24us     3.39ms
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
    Reqs/sec     18147.28    8223.24   64020.53
    Latency        2.75ms     2.62ms   228.29ms
    HTTP codes:
      1xx - 0, 2xx - 92382, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7618
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7618
    Throughput:     3.79MB/s
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
    Reqs/sec     19643.04    5925.88   77391.72
    Latency        2.54ms     1.92ms   167.75ms
    HTTP codes:
      1xx - 0, 2xx - 95753, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4247
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4247
    Throughput:     4.31MB/s
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
    Reqs/sec     69043.85    5636.13   87329.40
    Latency      718.14us   171.45us     7.37ms
    HTTP codes:
      1xx - 0, 2xx - 97516, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2484
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2484
    Throughput:    10.71MB/s
  ```


