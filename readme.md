## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72906` | `5473` | `81757` |
| **86%** | [Hyper Express](#hyper-express) | `62836` | `3641` | `65451` |
| **34%** | [Node (Default)](#node-default) | `24684` | `6976` | `53633` |
| **32%** | [Fastify](#fastify) | `23573` | `7134` | `35755` |
| **28%** | [Hono](#hono) | `20449` | `5519` | `29437` |
| **24%** | [Koa](#koa) | `17276` | `6025` | `54806` |
| **11%** | [Carbon](#carbon) | `8223` | `1381` | `10507` |
| **9%** | [Express](#express) | `6615` | `1078` | `8455` |


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
    Reqs/sec      8885.86    4552.67   62867.55
    Latency        5.61ms     4.07ms   346.60ms
    HTTP codes:
      1xx - 0, 2xx - 93377, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6623
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6623
    Throughput:     1.89MB/s
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
    Reqs/sec      6539.71    1061.03    8480.48
    Latency        7.64ms     3.60ms   346.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     23373.59    6923.63   35905.76
    Latency        2.14ms     2.00ms   179.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.30MB/s
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
    Reqs/sec     19865.11    5471.18   29598.87
    Latency        2.51ms     1.93ms   175.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.49MB/s
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
    Reqs/sec     63863.55    4811.97   86239.06
    Latency      780.80us    68.64us     4.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.07MB/s
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
    Reqs/sec     18061.62    6294.04   50011.05
    Latency        2.76ms     2.33ms   205.58ms
    HTTP codes:
      1xx - 0, 2xx - 94828, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5172
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5172
    Throughput:     3.88MB/s
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
    Reqs/sec     24349.43    6607.22   52858.59
    Latency        2.05ms     1.92ms   167.46ms
    HTTP codes:
      1xx - 0, 2xx - 97607, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2393
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2393
    Throughput:     5.43MB/s
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
    Reqs/sec     73719.53    5911.39   83794.80
    Latency      676.07us   288.23us    25.33ms
    HTTP codes:
      1xx - 0, 2xx - 97684, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2316
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2316
    Throughput:    11.39MB/s
  ```


