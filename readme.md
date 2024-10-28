## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76991` | `6901` | `98563` |
| **81%** | [Hyper Express](#hyper-express) | `62287` | `5782` | `81807` |
| **33%** | [Node (Default)](#node-default) | `25315` | `7700` | `63027` |
| **32%** | [Fastify](#fastify) | `24819` | `7386` | `37036` |
| **28%** | [Hono](#hono) | `21756` | `6105` | `30457` |
| **24%** | [Koa](#koa) | `18582` | `5692` | `47154` |
| **11%** | [Carbon](#carbon) | `8322` | `1417` | `10531` |
| **9%** | [Express](#express) | `6801` | `1126` | `8768` |


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
    Reqs/sec      9060.58    5484.24   62740.97
    Latency        5.51ms     4.11ms   357.32ms
    HTTP codes:
      1xx - 0, 2xx - 91203, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8797
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8797
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
    Reqs/sec      7202.49    3998.71   48241.56
    Latency        6.93ms     3.81ms   346.11ms
    HTTP codes:
      1xx - 0, 2xx - 92952, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7048
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7048
    Throughput:     1.92MB/s
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
    Reqs/sec     24457.84    6909.74   36115.07
    Latency        2.04ms     1.89ms   171.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.55MB/s
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
    Reqs/sec     21241.87    5352.11   30046.10
    Latency        2.35ms     1.96ms   176.60ms
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
    Reqs/sec     64123.90    3894.79   67516.10
    Latency      777.63us    63.16us     3.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.11MB/s
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
    Reqs/sec     18570.82    6498.93   62921.36
    Latency        2.68ms     2.14ms   188.90ms
    HTTP codes:
      1xx - 0, 2xx - 94353, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5647
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5647
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
    Reqs/sec     26695.35    7579.28   49930.40
    Latency        1.87ms     1.77ms   151.62ms
    HTTP codes:
      1xx - 0, 2xx - 97682, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2318
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2318
    Throughput:     5.97MB/s
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
    Reqs/sec     75733.02    5379.42   86736.82
    Latency      657.80us   204.45us    10.34ms
    HTTP codes:
      1xx - 0, 2xx - 96484, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3516
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3516
    Throughput:    11.55MB/s
  ```


