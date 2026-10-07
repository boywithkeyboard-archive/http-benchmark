## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `123676` | `5152` | `133298` |
| **90%** | [Hyper Express](#hyper-express) | `110761` | `5877` | `114057` |
| **55%** | [Node (Default)](#node-default) | `67591` | `15398` | `102962` |
| **54%** | [Fastify](#fastify) | `67275` | `16976` | `87122` |
| **51%** | [Hono](#hono) | `62559` | `16258` | `80209` |
| **45%** | [Koa](#koa) | `55509` | `16908` | `129212` |
| **19%** | [Carbon](#carbon) | `23634` | `6823` | `36018` |
| **13%** | [Express](#express) | `16680` | `3625` | `23627` |


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
    Reqs/sec     27052.43   15319.86  125988.20
    Latency        1.84ms     2.98ms   246.89ms
    HTTP codes:
      1xx - 0, 2xx - 89012, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10988
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10988
    Throughput:     5.47MB/s
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
    Reqs/sec     18704.47   12190.46  109586.74
    Latency        2.67ms     2.50ms   220.84ms
    HTTP codes:
      1xx - 0, 2xx - 89613, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10387
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10387
    Throughput:     4.79MB/s
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
    Reqs/sec     67711.09   19767.99  117940.69
    Latency      734.38us     0.94ms    78.72ms
    HTTP codes:
      1xx - 0, 2xx - 84236, 3xx - 0, 4xx - 0, 5xx - 0
      others - 15764
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 15752
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 12
    Throughput:    12.97MB/s
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
    Reqs/sec     63641.48   19646.59  123388.88
    Latency      783.33us     1.08ms    89.43ms
    HTTP codes:
      1xx - 0, 2xx - 92154, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7846
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7844
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:    13.26MB/s
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
    Reqs/sec    111226.37    6367.42  121148.40
    Latency      447.77us   162.67us     5.85ms
    HTTP codes:
      1xx - 0, 2xx - 93448, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6552
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6552
    Throughput:    14.78MB/s
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
    Reqs/sec     64328.31   20190.90  130253.25
    Latency      775.07us     1.16ms    96.78ms
    HTTP codes:
      1xx - 0, 2xx - 89450, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10550
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10550
    Throughput:    13.02MB/s
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
    Reqs/sec     71754.22   16575.67  120363.01
    Latency      695.26us     0.87ms    72.35ms
    HTTP codes:
      1xx - 0, 2xx - 96103, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3897
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3897
    Throughput:    15.78MB/s
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
    Reqs/sec    120703.00    4495.75  126760.72
    Latency      412.51us   152.27us     8.68ms
    HTTP codes:
      1xx - 0, 2xx - 94673, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5327
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5327
    Throughput:    18.08MB/s
  ```


