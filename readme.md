## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73799` | `5601` | `86099` |
| **84%** | [Hyper Express](#hyper-express) | `62195` | `4302` | `66899` |
| **36%** | [Node (Default)](#node-default) | `26573` | `7744` | `53454` |
| **32%** | [Fastify](#fastify) | `23768` | `7098` | `36529` |
| **28%** | [Hono](#hono) | `20579` | `5749` | `29419` |
| **25%** | [Koa](#koa) | `18798` | `6393` | `56641` |
| **11%** | [Carbon](#carbon) | `8212` | `1400` | `10346` |
| **9%** | [Express](#express) | `6498` | `1086` | `8451` |


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
    Reqs/sec      8667.27    4421.39   52353.14
    Latency        5.76ms     4.33ms   374.38ms
    HTTP codes:
      1xx - 0, 2xx - 94001, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5999
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5999
    Throughput:     1.85MB/s
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
    Reqs/sec      6461.16    1061.54    8422.06
    Latency        7.74ms     3.58ms   347.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24773.38    7568.93   37710.24
    Latency        2.02ms     2.03ms   181.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     21010.32    5590.94   30665.20
    Latency        2.38ms     1.96ms   178.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     62770.35    4483.02   87365.56
    Latency      798.30us    71.72us     3.02ms
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
    Reqs/sec     18987.15    6674.70   59935.35
    Latency        2.63ms     2.44ms   209.63ms
    HTTP codes:
      1xx - 0, 2xx - 93879, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6121
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6121
    Throughput:     4.03MB/s
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
    Reqs/sec     26355.09    7319.53   54500.48
    Latency        1.89ms     1.77ms   156.04ms
    HTTP codes:
      1xx - 0, 2xx - 97550, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2450
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2450
    Throughput:     5.89MB/s
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
    Reqs/sec     75336.84    5484.55   92564.57
    Latency      661.20us   145.98us     7.38ms
    HTTP codes:
      1xx - 0, 2xx - 97651, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2349
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2349
    Throughput:    11.64MB/s
  ```


