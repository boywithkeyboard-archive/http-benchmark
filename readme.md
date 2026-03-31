## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69935` | `4033` | `81140` |
| **82%** | [Hyper Express](#hyper-express) | `57543` | `3889` | `65937` |
| **29%** | [Hono](#hono) | `20256` | `5941` | `29040` |
| **29%** | [Node (Default)](#node-default) | `20224` | `5451` | `72711` |
| **28%** | [Fastify](#fastify) | `19276` | `4759` | `36886` |
| **26%** | [Koa](#koa) | `18120` | `8019` | `62591` |
| **10%** | [Carbon](#carbon) | `7297` | `1300` | `10064` |
| **9%** | [Express](#express) | `5957` | `1068` | `8086` |


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
    Reqs/sec      7859.58    6781.94   80266.12
    Latency        6.36ms     4.63ms   402.63ms
    HTTP codes:
      1xx - 0, 2xx - 88848, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11152
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11152
    Throughput:     1.58MB/s
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
    Reqs/sec      5924.18    1106.13    8372.65
    Latency        8.44ms     4.06ms   390.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.69MB/s
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
    Reqs/sec     20584.66    5982.44   34775.73
    Latency        2.43ms     2.28ms   203.85ms
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
    Reqs/sec     20987.14    6400.31   29278.80
    Latency        2.38ms     2.17ms   198.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     58963.33    3495.72   66332.86
    Latency      845.74us    85.21us     3.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.37MB/s
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
    Reqs/sec     18026.27    9462.86   76715.46
    Latency        2.77ms     2.64ms   232.03ms
    HTTP codes:
      1xx - 0, 2xx - 90474, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9526
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9526
    Throughput:     3.69MB/s
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
    Reqs/sec     20171.81    4675.22   53326.01
    Latency        2.47ms     2.16ms   187.76ms
    HTTP codes:
      1xx - 0, 2xx - 97822, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2178
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2178
    Throughput:     4.52MB/s
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
    Reqs/sec     68483.33    3321.69   78117.67
    Latency      727.64us   164.78us     8.53ms
    HTTP codes:
      1xx - 0, 2xx - 96755, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3245
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3245
    Throughput:    10.49MB/s
  ```


