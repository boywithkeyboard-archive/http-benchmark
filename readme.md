## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73843` | `2472` | `80067` |
| **84%** | [Hyper Express](#hyper-express) | `62200` | `3767` | `65537` |
| **36%** | [Node (Default)](#node-default) | `26685` | `9873` | `82261` |
| **33%** | [Fastify](#fastify) | `24307` | `7432` | `35576` |
| **26%** | [Hono](#hono) | `19561` | `5264` | `29102` |
| **25%** | [Koa](#koa) | `18547` | `8052` | `67248` |
| **11%** | [Carbon](#carbon) | `8051` | `1392` | `10334` |
| **9%** | [Express](#express) | `6314` | `999` | `8402` |


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
    Reqs/sec      9008.66    5905.71   66251.97
    Latency        5.54ms     4.19ms   362.99ms
    HTTP codes:
      1xx - 0, 2xx - 91667, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8333
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8333
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
    Reqs/sec      6949.45    5621.47   66750.33
    Latency        7.18ms     4.03ms   367.68ms
    HTTP codes:
      1xx - 0, 2xx - 90539, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9461
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9461
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
    Reqs/sec     24085.48    7548.63   36355.50
    Latency        2.07ms     2.13ms   188.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     20850.39    5203.98   29548.63
    Latency        2.40ms     2.08ms   185.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     62056.46    3038.02   70097.44
    Latency      803.80us    68.60us     2.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     18514.42    8390.48   76431.07
    Latency        2.69ms     2.44ms   214.50ms
    HTTP codes:
      1xx - 0, 2xx - 92171, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7829
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7829
    Throughput:     3.86MB/s
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
    Reqs/sec     26371.27    8434.76   62937.31
    Latency        1.89ms     1.85ms   160.15ms
    HTTP codes:
      1xx - 0, 2xx - 97439, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2561
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2561
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
    Reqs/sec     73489.90    2644.62   84373.20
    Latency      677.49us   140.14us     7.59ms
    HTTP codes:
      1xx - 0, 2xx - 96167, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3833
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3833
    Throughput:    11.19MB/s
  ```


