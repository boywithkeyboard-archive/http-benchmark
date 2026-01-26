## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74219` | `2962` | `81481` |
| **84%** | [Hyper Express](#hyper-express) | `61980` | `3656` | `65424` |
| **36%** | [Node (Default)](#node-default) | `26402` | `8094` | `57519` |
| **31%** | [Fastify](#fastify) | `23312` | `7101` | `36066` |
| **28%** | [Hono](#hono) | `20891` | `6033` | `29929` |
| **26%** | [Koa](#koa) | `19320` | `7789` | `65594` |
| **11%** | [Carbon](#carbon) | `8159` | `1473` | `10458` |
| **9%** | [Express](#express) | `6405` | `1056` | `8487` |


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
    Reqs/sec      8856.94    5222.87   67379.16
    Latency        5.63ms     4.26ms   364.21ms
    HTTP codes:
      1xx - 0, 2xx - 93084, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6916
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6916
    Throughput:     1.87MB/s
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
    Reqs/sec      7153.45    6973.40   76523.84
    Latency        6.97ms     3.95ms   361.36ms
    HTTP codes:
      1xx - 0, 2xx - 87232, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12768
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12768
    Throughput:     1.79MB/s
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
    Reqs/sec     24262.24    7527.36   39351.75
    Latency        2.06ms     1.92ms   175.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     21062.67    5941.09   31395.46
    Latency        2.37ms     1.94ms   176.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     63574.42    3666.43   66510.09
    Latency      784.64us    72.60us     3.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.03MB/s
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
    Reqs/sec     18879.77    8594.49   75897.06
    Latency        2.64ms     2.38ms   211.24ms
    HTTP codes:
      1xx - 0, 2xx - 92054, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7946
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7946
    Throughput:     3.93MB/s
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
    Reqs/sec     24478.53    8091.23   76800.49
    Latency        2.04ms     1.88ms   157.59ms
    HTTP codes:
      1xx - 0, 2xx - 96524, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3476
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3476
    Throughput:     5.41MB/s
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
    Reqs/sec     74398.74    3413.72   90689.74
    Latency      668.79us   180.99us    10.36ms
    HTTP codes:
      1xx - 0, 2xx - 96344, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3656
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3656
    Throughput:    11.35MB/s
  ```


