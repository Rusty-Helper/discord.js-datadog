# discord.js-datadog
Discord.js monitoring without limits

Bring the power of [DataDog](https://www.datadoghq.com/) to discord.js with fully intergrated and extensible metrics

Automatically collect and tag key metrics specific to discord.js

## Metrics we collect

| Metric name            | Description                                       |
|------------------------|---------------------------------------------------|
| shard.ws.ping          | Websocket heartbeat latency                       |
| shard.ws.status        | Last status of the websocket connection           |
| shard.count            | Number of shards the bot has                      |
| shard.guilds.count     | Number of guilds for each shard                   |
| shard.guilds.join      | Guilds left that shard                            |
| shard.guilds.leave     | Guilds joined that shard                          |
| shard.channels.count   | Number of channels for each shard                 |
| shard.events.seen      | Raw number of events that shard saw               |
| shard.events.handled   | Number of events that shard handled               |
| shard.messages.handled | Messages handled by each shard                    |
| shard.command.duration | Time spent responding to a command                |
| shard.command.error    | Errors from commands                              |
| shard.command.invalid  | Number of commands not completed by user          |
| client.messages.sent   | Messages sent by that client                      |
| client.rest.sent       | REST API calls made                               |
| client.rest.ratelimit  | Rate limits hit                                   |
| client.rest.ping       | REST API ping                                     |
| client.rest.failed     | REST API calls which failed with a 5XX error code |
| cli                    |                                                   |

## Logging

We recommend you use the winston logger.

## Traces

You can use dd-trace 