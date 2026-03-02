## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71166` | `4459` | `91988` |
| **84%** | [Hyper Express](#hyper-express) | `59961` | `3283` | `67215` |
| **33%** | [Node (Default)](#node-default) | `23692` | `5863` | `61198` |
| **32%** | [Fastify](#fastify) | `23077` | `6860` | `38236` |
| **30%** | [Hono](#hono) | `21468` | `6853` | `30985` |
| **25%** | [Koa](#koa) | `17920` | `7986` | `65931` |
| **11%** | [Carbon](#carbon) | `8140` | `1417` | `10327` |
| **9%** | [Express](#express) | `6461` | `1120` | `8328` |


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
    Reqs/sec      8675.58    5410.67   69657.05
    Latency        5.75ms     4.32ms   372.55ms
    HTTP codes:
      1xx - 0, 2xx - 92340, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7660
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7660
    Throughput:     1.82MB/s
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
    Reqs/sec      6286.39    1031.95    8329.77
    Latency        7.95ms     3.76ms   358.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     24037.74    7254.70   38125.88
    Latency        2.08ms     1.98ms   181.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     21276.46    6333.36   30713.84
    Latency        2.35ms     2.00ms   179.07ms
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
    Reqs/sec     59557.30    3201.67   68018.78
    Latency      838.65us    81.29us     2.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.45MB/s
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
    Reqs/sec     18707.13    7685.13   61780.59
    Latency        2.67ms     2.46ms   218.50ms
    HTTP codes:
      1xx - 0, 2xx - 93794, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6206
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6206
    Throughput:     3.97MB/s
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
    Reqs/sec     23797.49    5653.40   55659.59
    Latency        2.10ms     1.91ms   167.07ms
    HTTP codes:
      1xx - 0, 2xx - 97640, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2360
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2360
    Throughput:     5.31MB/s
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
    Reqs/sec     71733.85    3621.64   78910.79
    Latency      693.42us   205.54us    12.89ms
    HTTP codes:
      1xx - 0, 2xx - 96266, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3734
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3734
    Throughput:    10.93MB/s
  ```


