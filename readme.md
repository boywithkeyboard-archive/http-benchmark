## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70136` | `4223` | `83702` |
| **83%** | [Hyper Express](#hyper-express) | `58356` | `3731` | `67480` |
| **30%** | [Hono](#hono) | `21108` | `6785` | `30345` |
| **29%** | [Koa](#koa) | `20095` | `8531` | `64382` |
| **29%** | [Node (Default)](#node-default) | `20028` | `5387` | `64022` |
| **28%** | [Fastify](#fastify) | `19807` | `4529` | `34642` |
| **10%** | [Carbon](#carbon) | `7313` | `1300` | `10393` |
| **9%** | [Express](#express) | `6064` | `1035` | `8178` |


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
    Reqs/sec      7829.58    5463.77   74314.83
    Latency        6.38ms     4.76ms   398.54ms
    HTTP codes:
      1xx - 0, 2xx - 91667, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8333
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8333
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
    Reqs/sec      5998.18    1027.36    8149.39
    Latency        8.33ms     3.90ms   374.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.72MB/s
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
    Reqs/sec     20882.29    5617.03   36001.45
    Latency        2.39ms     2.20ms   198.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     20703.16    6721.15   30038.90
    Latency        2.41ms     2.48ms   211.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     57977.04    3596.40   66254.45
    Latency        0.86ms   100.46us     4.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.24MB/s
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
    Reqs/sec     18781.78    7665.41   61922.54
    Latency        2.65ms     2.50ms   221.40ms
    HTTP codes:
      1xx - 0, 2xx - 93583, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6417
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6417
    Throughput:     3.98MB/s
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
    Reqs/sec     19946.34    4485.49   54680.74
    Latency        2.50ms     2.01ms   173.18ms
    HTTP codes:
      1xx - 0, 2xx - 97445, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2555
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2555
    Throughput:     4.45MB/s
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
    Reqs/sec     70338.74    5805.29   85297.50
    Latency      706.81us   205.26us    11.69ms
    HTTP codes:
      1xx - 0, 2xx - 96076, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3924
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3924
    Throughput:    10.70MB/s
  ```


