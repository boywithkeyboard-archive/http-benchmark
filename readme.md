## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73945` | `5184` | `80395` |
| **86%** | [Hyper Express](#hyper-express) | `63536` | `3943` | `67015` |
| **37%** | [Node (Default)](#node-default) | `27386` | `8442` | `51989` |
| **34%** | [Fastify](#fastify) | `25490` | `7919` | `37458` |
| **29%** | [Hono](#hono) | `21474` | `5998` | `30221` |
| **26%** | [Koa](#koa) | `19045` | `6937` | `52875` |
| **11%** | [Carbon](#carbon) | `8239` | `1352` | `10287` |
| **9%** | [Express](#express) | `6520` | `1028` | `8482` |


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
    Reqs/sec      8648.78    4291.52   56666.17
    Latency        5.77ms     4.25ms   367.79ms
    HTTP codes:
      1xx - 0, 2xx - 94141, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5859
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5859
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
    Reqs/sec      7083.02    4890.96   54510.40
    Latency        7.05ms     3.90ms   355.54ms
    HTTP codes:
      1xx - 0, 2xx - 91128, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8872
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8872
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
    Reqs/sec     24685.86    7163.94   37489.13
    Latency        2.02ms     1.91ms   173.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.60MB/s
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
    Reqs/sec     21048.07    5897.93   31143.56
    Latency        2.38ms     1.98ms   179.11ms
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
    Reqs/sec     64793.88    4093.50   72551.14
    Latency      770.49us    72.55us     3.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.19MB/s
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
    Reqs/sec     19247.43    7063.13   59458.50
    Latency        2.59ms     2.37ms   207.40ms
    HTTP codes:
      1xx - 0, 2xx - 92559, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7441
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7441
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
    Reqs/sec     27612.20    8645.18   49666.83
    Latency        1.81ms     1.89ms   160.66ms
    HTTP codes:
      1xx - 0, 2xx - 97758, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2242
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2242
    Throughput:     6.18MB/s
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
    Reqs/sec     74441.58    5034.10   84789.93
    Latency      668.82us   152.77us     5.28ms
    HTTP codes:
      1xx - 0, 2xx - 96942, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3058
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3058
    Throughput:    11.43MB/s
  ```


