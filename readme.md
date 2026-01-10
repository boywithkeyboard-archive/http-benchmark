## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76050` | `2735` | `86818` |
| **84%** | [Hyper Express](#hyper-express) | `64045` | `4152` | `67697` |
| **34%** | [Node (Default)](#node-default) | `25984` | `8185` | `70822` |
| **31%** | [Fastify](#fastify) | `23626` | `7591` | `35956` |
| **27%** | [Hono](#hono) | `20403` | `5706` | `29571` |
| **25%** | [Koa](#koa) | `18842` | `8295` | `67187` |
| **10%** | [Carbon](#carbon) | `7746` | `1330` | `9716` |
| **8%** | [Express](#express) | `6107` | `1000` | `8003` |


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
    Reqs/sec      8759.46    7289.53   80538.25
    Latency        5.70ms     4.49ms   388.31ms
    HTTP codes:
      1xx - 0, 2xx - 88684, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11316
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11316
    Throughput:     1.76MB/s
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
    Reqs/sec      6665.85    6196.09   80286.09
    Latency        7.49ms     4.08ms   370.49ms
    HTTP codes:
      1xx - 0, 2xx - 88972, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11028
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11028
    Throughput:     1.70MB/s
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
    Reqs/sec     24201.21    7389.20   36440.31
    Latency        2.06ms     2.06ms   183.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     20049.49    5380.66   29470.40
    Latency        2.49ms     2.14ms   189.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.53MB/s
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
    Reqs/sec     64459.30    3276.67   67633.10
    Latency      773.55us    62.20us     2.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.15MB/s
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
    Reqs/sec     19081.98    9057.96   77408.17
    Latency        2.62ms     2.43ms   216.04ms
    HTTP codes:
      1xx - 0, 2xx - 91298, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8702
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8702
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
    Reqs/sec     26756.52    8870.36   79218.99
    Latency        1.86ms     2.06ms   175.45ms
    HTTP codes:
      1xx - 0, 2xx - 96575, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3425
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3425
    Throughput:     5.92MB/s
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
    Reqs/sec     75770.34    2098.65   78589.08
    Latency      656.84us   157.02us     9.86ms
    HTTP codes:
      1xx - 0, 2xx - 96658, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3342
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3342
    Throughput:    11.59MB/s
  ```


