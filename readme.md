## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70535` | `4060` | `80723` |
| **84%** | [Hyper Express](#hyper-express) | `59309` | `3570` | `65564` |
| **31%** | [Hono](#hono) | `21931` | `7013` | `30925` |
| **30%** | [Node (Default)](#node-default) | `20854` | `6249` | `74244` |
| **29%** | [Fastify](#fastify) | `20276` | `5200` | `36510` |
| **24%** | [Koa](#koa) | `17053` | `7356` | `60010` |
| **10%** | [Carbon](#carbon) | `7228` | `1173` | `10161` |
| **9%** | [Express](#express) | `6166` | `1075` | `8183` |


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
    Reqs/sec      8288.11    6933.84   71918.95
    Latency        6.02ms     4.47ms   382.23ms
    HTTP codes:
      1xx - 0, 2xx - 87953, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12047
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12047
    Throughput:     1.66MB/s
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
    Reqs/sec      6133.27    1050.74    8141.01
    Latency        8.15ms     3.82ms   367.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     20550.93    5280.29   36991.92
    Latency        2.43ms     2.15ms   191.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
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
    Reqs/sec     20757.64    6321.07   30902.48
    Latency        2.41ms     2.11ms   186.57ms
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
    Reqs/sec     59275.42    4144.02   65650.57
    Latency      841.10us    97.35us     5.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.42MB/s
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
    Reqs/sec     16533.55    7622.93   64812.46
    Latency        3.01ms     2.49ms   217.32ms
    HTTP codes:
      1xx - 0, 2xx - 93001, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6999
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6999
    Throughput:     3.48MB/s
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
    Reqs/sec     20871.04    5595.64   75401.89
    Latency        2.38ms     1.93ms   163.72ms
    HTTP codes:
      1xx - 0, 2xx - 96616, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3384
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3384
    Throughput:     4.63MB/s
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
    Reqs/sec     70167.30    3316.32   82862.41
    Latency      709.16us   202.43us    11.28ms
    HTTP codes:
      1xx - 0, 2xx - 96345, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3655
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3655
    Throughput:    10.70MB/s
  ```


