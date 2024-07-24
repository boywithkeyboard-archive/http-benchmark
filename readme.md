## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73744` | `4557` | `79397` |
| **86%** | [Hyper Express](#hyper-express) | `63680` | `4217` | `77968` |
| **35%** | [Node (Default)](#node-default) | `25571` | `7782` | `58306` |
| **32%** | [Fastify](#fastify) | `23708` | `7061` | `35886` |
| **26%** | [Hono](#hono) | `19538` | `5340` | `28680` |
| **24%** | [Koa](#koa) | `17759` | `6574` | `54915` |
| **11%** | [Carbon](#carbon) | `8027` | `1378` | `10239` |
| **9%** | [Express](#express) | `6463` | `1030` | `8400` |


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
    Reqs/sec      8473.24    4379.83   59371.32
    Latency        5.89ms     4.18ms   358.40ms
    HTTP codes:
      1xx - 0, 2xx - 93494, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6506
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6506
    Throughput:     1.80MB/s
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
    Reqs/sec      6484.93    1049.92    8316.45
    Latency        7.71ms     3.57ms   345.29ms
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
    Reqs/sec     23294.74    6979.95   36341.92
    Latency        2.14ms     2.01ms   183.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.29MB/s
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
    Reqs/sec     20053.23    5293.32   29203.59
    Latency        2.49ms     1.94ms   175.98ms
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
    Reqs/sec     64281.08    3591.76   67480.08
    Latency      775.82us    60.33us     2.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.13MB/s
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
    Reqs/sec     17566.73    6032.16   58734.09
    Latency        2.84ms     2.43ms   213.82ms
    HTTP codes:
      1xx - 0, 2xx - 94315, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5685
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5685
    Throughput:     3.74MB/s
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
    Reqs/sec     26205.98    8381.84   41725.41
    Latency        1.91ms     1.92ms   167.11ms
    HTTP codes:
      1xx - 0, 2xx - 98389, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1611
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1611
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
    Reqs/sec     74423.12    6436.71   80061.81
    Latency      670.24us   263.01us    18.62ms
    HTTP codes:
      1xx - 0, 2xx - 97118, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2882
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2880
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:    11.42MB/s
  ```


