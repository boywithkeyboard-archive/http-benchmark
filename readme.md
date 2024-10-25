## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73604` | `7756` | `86267` |
| **84%** | [Hyper Express](#hyper-express) | `62186` | `3479` | `65352` |
| **36%** | [Node (Default)](#node-default) | `26314` | `7164` | `43841` |
| **32%** | [Fastify](#fastify) | `23831` | `7417` | `35498` |
| **27%** | [Hono](#hono) | `20192` | `5180` | `28955` |
| **25%** | [Koa](#koa) | `18334` | `6531` | `58809` |
| **11%** | [Carbon](#carbon) | `8286` | `1378` | `10485` |
| **9%** | [Express](#express) | `6529` | `1018` | `8586` |


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
    Reqs/sec      8516.30    3030.42   41906.44
    Latency        5.86ms     4.20ms   365.26ms
    HTTP codes:
      1xx - 0, 2xx - 96071, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3929
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3929
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
    Reqs/sec      6581.23    1033.37    8546.21
    Latency        7.60ms     3.59ms   342.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     24322.43    7179.51   35855.42
    Latency        2.05ms     2.00ms   182.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     20500.57    5583.26   28922.73
    Latency        2.44ms     1.87ms   172.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     62294.79    3283.40   67393.68
    Latency      799.23us    73.86us     4.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.86MB/s
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
    Reqs/sec     18735.32    6198.36   49419.46
    Latency        2.66ms     2.39ms   211.38ms
    HTTP codes:
      1xx - 0, 2xx - 94720, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5280
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5280
    Throughput:     4.01MB/s
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
    Reqs/sec     25833.54    7952.07   62142.54
    Latency        1.93ms     1.88ms   161.41ms
    HTTP codes:
      1xx - 0, 2xx - 97282, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2718
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2718
    Throughput:     5.75MB/s
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
    Reqs/sec     73684.10    6004.19   89733.65
    Latency      678.05us   194.18us    11.64ms
    HTTP codes:
      1xx - 0, 2xx - 97476, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2524
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2524
    Throughput:    11.33MB/s
  ```


