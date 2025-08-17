## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73390` | `2448` | `79488` |
| **82%** | [Hyper Express](#hyper-express) | `60499` | `3144` | `64196` |
| **34%** | [Node (Default)](#node-default) | `25034` | `7885` | `63958` |
| **32%** | [Fastify](#fastify) | `23201` | `7080` | `34932` |
| **28%** | [Hono](#hono) | `20468` | `5734` | `30179` |
| **26%** | [Koa](#koa) | `18930` | `9176` | `70470` |
| **11%** | [Carbon](#carbon) | `7951` | `1360` | `10220` |
| **9%** | [Express](#express) | `6373` | `1089` | `8305` |


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
    Reqs/sec      9058.66    6407.98   83164.27
    Latency        5.51ms     4.27ms   368.72ms
    HTTP codes:
      1xx - 0, 2xx - 91168, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8832
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8832
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
    Reqs/sec      7055.36    6374.45   84150.54
    Latency        7.08ms     4.02ms   369.85ms
    HTTP codes:
      1xx - 0, 2xx - 89461, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10539
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10539
    Throughput:     1.81MB/s
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
    Reqs/sec     22825.83    7051.74   36260.81
    Latency        2.19ms     2.08ms   185.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.17MB/s
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
    Reqs/sec     20600.92    5777.33   29306.47
    Latency        2.43ms     2.05ms   185.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     61154.87    3134.17   64220.06
    Latency      815.21us    64.83us     2.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.69MB/s
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
    Reqs/sec     18860.74    8441.16   72048.00
    Latency        2.64ms     2.43ms   222.51ms
    HTTP codes:
      1xx - 0, 2xx - 92808, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7192
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7192
    Throughput:     3.96MB/s
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
    Reqs/sec     25666.28    9386.89   81552.45
    Latency        1.95ms     1.91ms   163.33ms
    HTTP codes:
      1xx - 0, 2xx - 95640, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4360
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4360
    Throughput:     5.62MB/s
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
    Reqs/sec     73638.92    2556.02   81626.13
    Latency      676.73us   112.22us     7.70ms
    HTTP codes:
      1xx - 0, 2xx - 97530, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2470
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2470
    Throughput:    11.37MB/s
  ```


