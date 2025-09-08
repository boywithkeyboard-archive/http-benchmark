## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73325` | `3152` | `78689` |
| **85%** | [Hyper Express](#hyper-express) | `62412` | `4241` | `73643` |
| **34%** | [Node (Default)](#node-default) | `25081` | `8070` | `70777` |
| **32%** | [Fastify](#fastify) | `23356` | `6990` | `34742` |
| **27%** | [Hono](#hono) | `19616` | `5513` | `29289` |
| **25%** | [Koa](#koa) | `18241` | `8549` | `67430` |
| **11%** | [Carbon](#carbon) | `8114` | `1424` | `10321` |
| **9%** | [Express](#express) | `6340` | `1058` | `8368` |


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
    Reqs/sec      8711.01    6897.70   81771.82
    Latency        5.73ms     4.41ms   378.03ms
    HTTP codes:
      1xx - 0, 2xx - 89011, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10989
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10989
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
    Reqs/sec      6303.09    1069.36    8334.91
    Latency        7.93ms     3.71ms   355.27ms
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
    Reqs/sec     23516.04    6754.73   34627.94
    Latency        2.12ms     2.05ms   183.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.34MB/s
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
    Reqs/sec     20975.15    5837.74   28999.43
    Latency        2.38ms     2.03ms   183.07ms
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
    Reqs/sec     63325.07    3904.28   70388.57
    Latency      788.34us    71.94us     2.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     18810.24    8570.42   76263.27
    Latency        2.65ms     2.38ms   208.88ms
    HTTP codes:
      1xx - 0, 2xx - 92391, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7609
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7609
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
    Reqs/sec     25620.47    8550.03   73160.79
    Latency        1.95ms     2.02ms   170.53ms
    HTTP codes:
      1xx - 0, 2xx - 96453, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3547
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3547
    Throughput:     5.66MB/s
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
    Reqs/sec     74182.80    2903.08   85500.49
    Latency      670.78us   187.00us    11.63ms
    HTTP codes:
      1xx - 0, 2xx - 96293, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3707
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3707
    Throughput:    11.30MB/s
  ```


