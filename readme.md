## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71054` | `4471` | `82559` |
| **83%** | [Hyper Express](#hyper-express) | `58670` | `3536` | `65701` |
| **30%** | [Hono](#hono) | `21212` | `6991` | `30267` |
| **29%** | [Node (Default)](#node-default) | `20321` | `7122` | `78077` |
| **28%** | [Fastify](#fastify) | `20039` | `5318` | `35202` |
| **25%** | [Koa](#koa) | `18021` | `8212` | `63445` |
| **10%** | [Carbon](#carbon) | `7450` | `1359` | `10371` |
| **9%** | [Express](#express) | `6190` | `1117` | `8292` |


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
    Reqs/sec      7941.21    5997.84   74760.89
    Latency        6.29ms     4.68ms   398.82ms
    HTTP codes:
      1xx - 0, 2xx - 90158, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9842
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9842
    Throughput:     1.63MB/s
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
    Reqs/sec      6342.79    1117.01    8349.58
    Latency        7.88ms     3.85ms   369.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     20822.40    5511.06   36367.19
    Latency        2.40ms     2.16ms   192.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     21489.30    6635.14   30898.19
    Latency        2.32ms     2.20ms   194.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     58191.29    3650.40   63572.39
    Latency        0.86ms   107.76us     4.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.26MB/s
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
    Reqs/sec     19109.34    8245.58   62618.01
    Latency        2.61ms     2.48ms   218.46ms
    HTTP codes:
      1xx - 0, 2xx - 93464, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6536
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6536
    Throughput:     4.04MB/s
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
    Reqs/sec     20676.67    5039.07   55829.97
    Latency        2.42ms     2.09ms   180.69ms
    HTTP codes:
      1xx - 0, 2xx - 97413, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2587
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2587
    Throughput:     4.60MB/s
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
    Reqs/sec     70417.09    4690.65   82618.57
    Latency      707.26us   207.27us    10.88ms
    HTTP codes:
      1xx - 0, 2xx - 96645, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3355
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3355
    Throughput:    10.77MB/s
  ```


