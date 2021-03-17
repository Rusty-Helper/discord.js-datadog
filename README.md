# discord.js-datadog
Discord.js monitoring without limits

Bring the power of [DataDog](https://www.datadoghq.com/) to discord.js with fully intergrated and extensible metrics

Automatically collect and tag key metrics specific to discord.js

## Default metrics

| Metric name                 | Description                                            | Reports               | Metric type | Best display option |
|-----------------------------|--------------------------------------------------------|-----------------------|-------------|---------------------|
| shard.ws.ping               | Websocket heartbeat latency                            | Every 41250ms         | Counter     | Timeseries          |
| shard.ws.status             | Last status of the websocket connection                | Every 41250ms         | Counter     | Timeseries          |
| shard.count                 | Number of shards the bot has                           | Once on startup       | Counter     | Last count          |
| shard.guilds.count          | Number of guild in each shard                          | On guild leave + join | Counter     | Last count          |
| shard.guilds.join           | Guilds left that shard                                 | On guild leave        | Gauge       | Timeseries          |
| shard.guilds.join.members   | Size of guilds that join                               | On guild leave        | Gauge       | Timeseries          |
| shard.guilds.leave          | Guilds joined that shard                               | On guild join         | Gauge       | Timeseries          |
| shard.guilds.leave.members  | Size of guilds that leave                              | On guild join         | Gauge       | Timeseries          |
| shard.guilds.leave.duration | Length of time that guilds had your bot in             | On guild leave + join | Counter     | Last count Avg.     |
| shard.members.count         | Total guild members in each shard                      | Every 41250ms         | Counter     | Timeseries          |
| shard.channels.count        | Number of channels in each shard                       | Every 41250ms         | Counter     | Last count          |
| shard.events.seen           | Raw number of events that shard saw                    | Every 41250ms         | Counter     | Timeseries          |
| shard.events.handled        | Number of events that shard handled                    | Every 41250ms         | Counter     | Timeseries          |
| shard.messages.handled      | Messages handled by each shard                         | Every 41250ms         | Counter     | Timeseries          |
| shard.messages.bot          | Messages handled from bots                             | Every 41250ms         | Counter     | Timeseries          |
| shard.command.duration      | Time spent responding to a command                     | On command trigger    | Counter     | Toplist             |
| shard.command.error         | Errors from commands                                   | On command trigger    | Counter     | Toplist             |
| shard.command.invalid       | Number of commands not completed by user               | On command trigger    | Counter     | Toplist             |
| client.messages.sent        | Messages sent by that client                           | Every 41250ms         | Counter     | Timeseries          |
| client.rest.sent            | REST API calls made                                    | Every 41250ms         | Counter     | Timeseries          |
| client.rest.ratelimit       | Rate limits hit                                        | Every 41250ms         | Counter     | Timeseries          |
| client.rest.ping            | REST API ping                                          | Every 41250ms         | Counter     | Timeseries          |
| client.rest.failed          | REST API calls which failed with a 5XX error code      | Every 41250ms         | Counter     | Timeseries          |
| client.message.swept        | Total amount of messages swept                         | On message sweep      | Counter     | Timeseries          |
| client.cache.messages       | Number of messages in cache                            | Every 41250ms         | Counter     | Timeseries          |
| client.channels.swept       | Total amount of channels swept*                        | On channels sweep     | Counter     | Timeseries          |
| client.cache.channels       | Number of channels in cache**                          | Every 41250ms         | Counter     | Timeseries          |
| client.members.swept        | Total amount of members swept*                         | On users sweep        | Counter     | Timeseries          |
| client.cache.members        | Number of users in cache                               | Every 41250ms         | Counter     | Timeseries          |

*discord.js-light only
**meant discord.js-light as other metrics collect the same data for discord.js

All of these metrics are configurable in the initialisation options of the metric collector.

Access them through their metric name in the metric property of the options. Each of the metrics can be configured by an object or a number

If a number is provided it will override the default reporting internal. If an object is provided it will override some of the properties
like metric name, report internal, metric type and whether to report that metric and if a a falsy value is provided it will no longer
report that metric.

These options can be dynamically altered through `client.DDmetrics.options`

### Custom metrics

You can also provide custom metrics by adding your own properties or overriding the default ones.

The property name will serve as the metric name

You can of course provide a function as a metric and it's return value will be used as the value

## Tags

| Tag name | Default value |
|----------|---------------|
| process  | "bot"         |
| source   | "nodejs"      |
| shard    | shard id      |

### Custom tags

You can modify these tags with the `tag` object in the options as well as add your own tags

## Prefixes

Specify a prefix in the option and all metrics will be sent with that prefix. Don't include a dot . as it is automatically added.

e.g. 
`prefix: "mybot"`
`metirc: "client.ping"`

metric name in datadog will be `mybot.client.ping`

## Example

```js
const Discord = require("discord.js")
const DDmetrics = require("discord.js-datadog")
const client = new Discord.Client({ 
    // Discord.js client options here
})

client.datadog = DDmetrics({
    metrics: {
        "shard.ws.ping": {
            name: "shard.ping",
            reports: 2000
        },
        "shard.guilds.join": 6000,
        "shard.guilds.leave.duration": false,
        "process.memory.useage": process.memoryUsage().heapUsed
    },
    tags: {
        version: "v1",
        type: "bot"
    },
    prefix: "mybot",
    apiKey: "DD_API_KEY",
    interval: 10000,
})
```

## DataDog api 

We use [datadog-metrics](https://www.npmjs.com/package/datadog-metrics) for posting metrics to DataDog using their api.

**NO AGENT IS REQUIRED** this is not a StatsD library so we do not require an agent to post to

## Logging

DataDog recommends you use the [winston logger](https://github.com/winstonjs/winston)

Find configuration [here](https://docs.datadoghq.com/logs/log_collection/nodejs/?tab=winston30#configure-your-logger)

## Traces

DataDog recommends to use [dd-trace](https://datadoghq.dev/dd-trace-js/) for their custom [tracing](https://docs.datadoghq.com/tracing/)

## Contributing

Feel free to make a pull request with any features you want to see intergrated or make an issue with a bug.

All are welcome

# API

### Initialization


### Flush

`metrics.flush()`

Sends all currently stored metrics. You should not need to call this manually unless you have the interval set to 0

### DataDog metrics

You can find access to the `datadog-metrics` object through the `ddmetrics` property which will provide you full access
to all of the api properties documented for the [datadog-metrics](https://github.com/dbader/node-datadog-metrics) library