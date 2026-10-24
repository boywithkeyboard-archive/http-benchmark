## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `181218` | `6817` | `188067` |
| **84%** | [Hyper Express](#hyper-express) | `151710` | `10017` | `164006` |
| **35%** | [Node (Default)](#node-default) | `63503` | `16149` | `157552` |
| **32%** | [Fastify](#fastify) | `58859` | `10920` | `87347` |
| **29%** | [Koa](#koa) | `53024` | `21167` | `155256` |
| **27%** | [Hono](#hono) | `48886` | `8915` | `75743` |
| **13%** | [Carbon](#carbon) | `22863` | `5179` | `31657` |
| **9%** | [Express](#express) | `16176` | `2758` | `21221` |


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
    Reqs/sec     25706.54   18709.12  170106.72
    Latency        1.94ms     2.90ms   235.24ms
    HTTP codes:
      1xx - 0, 2xx - 87391, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12609
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12609
    Throughput:     5.11MB/s
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
    Reqs/sec     18626.91   18027.11  172460.90
    Latency        2.68ms     2.16ms   192.38ms
    HTTP codes:
      1xx - 0, 2xx - 85524, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14476
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14476
    Throughput:     4.56MB/s
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
    Reqs/sec     60406.00   27119.94  172676.51
    Latency      823.05us   716.59us    52.36ms
    HTTP codes:
      1xx - 0, 2xx - 80138, 3xx - 0, 4xx - 0, 5xx - 0
      others - 19862
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 19862
    Throughput:    11.01MB/s
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
    Reqs/sec     46422.40   17998.47  141099.53
    Latency        1.07ms     1.01ms    81.51ms
    HTTP codes:
      1xx - 0, 2xx - 89693, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10307
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10307
    Throughput:     9.43MB/s
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
    Reqs/sec    149923.33   10357.68  164536.58
    Latency      330.75us   186.86us     5.97ms
    HTTP codes:
      1xx - 0, 2xx - 90240, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9760
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9760
    Throughput:    19.23MB/s
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
    Reqs/sec     52616.75   25617.03  179173.83
    Latency        0.95ms     1.04ms    82.94ms
    HTTP codes:
      1xx - 0, 2xx - 86840, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13160
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13160
    Throughput:    10.35MB/s
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
    Reqs/sec     64976.01   14923.16  152971.00
    Latency      766.29us   823.50us    61.85ms
    HTTP codes:
      1xx - 0, 2xx - 95939, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4061
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4061
    Throughput:    14.29MB/s
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
    Reqs/sec    180333.88    9517.80  188337.40
    Latency      275.02us   108.48us     5.52ms
    HTTP codes:
      1xx - 0, 2xx - 95460, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4540
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4540
    Throughput:    27.24MB/s
  ```


