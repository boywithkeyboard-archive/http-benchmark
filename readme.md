## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `133492` | `7525` | `140733` |
| **87%** | [Hyper Express](#hyper-express) | `115717` | `6676` | `119989` |
| **33%** | [Node (Default)](#node-default) | `44028` | `12628` | `111110` |
| **31%** | [Fastify](#fastify) | `41256` | `7625` | `59178` |
| **29%** | [Koa](#koa) | `38491` | `17947` | `126909` |
| **28%** | [Hono](#hono) | `37572` | `8206` | `51121` |
| **13%** | [Carbon](#carbon) | `16965` | `4153` | `24855` |
| **9%** | [Express](#express) | `11586` | `2057` | `16323` |


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
    Reqs/sec     19212.69   15943.40  128237.22
    Latency        2.59ms     3.60ms   294.64ms
    HTTP codes:
      1xx - 0, 2xx - 85360, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14640
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14640
    Throughput:     3.74MB/s
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
    Reqs/sec     13334.41   14075.39  131106.76
    Latency        3.73ms     2.83ms   256.47ms
    HTTP codes:
      1xx - 0, 2xx - 85020, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14980
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14980
    Throughput:     3.25MB/s
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
    Reqs/sec     47260.94   23329.36  133816.34
    Latency        1.05ms     1.18ms    94.36ms
    HTTP codes:
      1xx - 0, 2xx - 76165, 3xx - 0, 4xx - 0, 5xx - 0
      others - 23835
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 23835
    Throughput:     8.18MB/s
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
    Reqs/sec     41591.57   19830.25  135953.97
    Latency        1.20ms     1.44ms   122.75ms
    HTTP codes:
      1xx - 0, 2xx - 86057, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13943
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13943
    Throughput:     8.11MB/s
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
    Reqs/sec    115219.91    7676.68  134738.08
    Latency      431.26us   211.15us     6.70ms
    HTTP codes:
      1xx - 0, 2xx - 88249, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11751
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11751
    Throughput:    14.44MB/s
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
    Reqs/sec     37096.42   16082.08  126029.74
    Latency        1.34ms     1.41ms   116.40ms
    HTTP codes:
      1xx - 0, 2xx - 89379, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10621
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10621
    Throughput:     7.50MB/s
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
    Reqs/sec     42909.72   12874.94  136945.55
    Latency        1.16ms     1.14ms    84.20ms
    HTTP codes:
      1xx - 0, 2xx - 93960, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6040
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6040
    Throughput:     9.25MB/s
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
    Reqs/sec    135063.64    3958.18  141037.15
    Latency      367.43us   153.80us     6.59ms
    HTTP codes:
      1xx - 0, 2xx - 92801, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7199
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7199
    Throughput:    19.84MB/s
  ```


