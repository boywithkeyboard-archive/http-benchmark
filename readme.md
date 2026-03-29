## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71327` | `5626` | `92641` |
| **84%** | [Hyper Express](#hyper-express) | `59838` | `3795` | `67453` |
| **32%** | [Hono](#hono) | `22726` | `7641` | `32134` |
| **29%** | [Fastify](#fastify) | `20796` | `5851` | `37909` |
| **29%** | [Koa](#koa) | `20437` | `11526` | `81358` |
| **28%** | [Node (Default)](#node-default) | `20015` | `5271` | `70173` |
| **10%** | [Carbon](#carbon) | `7316` | `1160` | `10508` |
| **9%** | [Express](#express) | `6157` | `1075` | `8223` |


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
    Reqs/sec      7996.70    6030.08   78499.12
    Latency        6.24ms     4.58ms   390.86ms
    HTTP codes:
      1xx - 0, 2xx - 91001, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8999
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8999
    Throughput:     1.65MB/s
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
    Reqs/sec      6074.99    1062.23    8166.37
    Latency        8.23ms     3.85ms   364.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     22487.88    6907.58   37314.19
    Latency        2.22ms     2.10ms   191.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.10MB/s
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
    Reqs/sec     21575.21    6888.61   32618.94
    Latency        2.32ms     2.09ms   187.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.87MB/s
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
    Reqs/sec     60295.93    4333.81   71170.56
    Latency      826.95us    93.89us     3.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.57MB/s
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
    Reqs/sec     20560.69    9018.20   62296.63
    Latency        2.42ms     2.45ms   210.85ms
    HTTP codes:
      1xx - 0, 2xx - 92825, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7175
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7175
    Throughput:     4.32MB/s
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
    Reqs/sec     20126.37    5282.43   67718.31
    Latency        2.48ms     2.07ms   182.07ms
    HTTP codes:
      1xx - 0, 2xx - 96653, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3347
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3347
    Throughput:     4.46MB/s
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
    Reqs/sec     70594.63    3960.58   83204.78
    Latency      704.64us   198.56us    10.64ms
    HTTP codes:
      1xx - 0, 2xx - 96436, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3564
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3564
    Throughput:    10.78MB/s
  ```


