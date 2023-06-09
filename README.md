# datadog-metrics

## Format

| API             | Syntax                                         | Description                            |
| --------------- | ---------------------------------------------- | -------------------------------------- |
| Initialization  |<details open> <summary> `metrics.init(options)` </summary> </br> <table>  <thead>  <tr>  <th>Option</th>  <th>Description</th>  </tr>  </thead>  <tbody>  <tr> <td><code>host</code></td>  <td>Sets host.</td>  <tr> <td><code>prefix</code></td>  <td>Sets prefix.</td> <tr> <td><code>flushIntervalSeconds</code></td>  <td>Sets how often to send metrics. Default is 15 seconds.</td> <tr> <td><code>site</code></td>  <td>Sets site. Default is `datadoghq.com`</td> <tr> <td><code>apiKey</code></td>  <td>Sets API key. Best practice to keep as environment variable.</td> <tr> <td><code>appKey</code></td>  <td>Sets app key. Best practice to keep as environment variable.</td> <tr> <td><code>defaultTags</code></td>  <td>Sets tags for metrics.</td> <tr> <td><code>onError</code></td>  <td>Handle asynchronous errors sending buffered metrics. Takes optional argument (the error).</td> <tr> <td><code>histogram</code></td>  <td>Same properties as `histogram()` method</td> <tr> <td><code>reporter</code></td>  <td>Sends the buffered metrics. Default is `reporters.DatadogReporter` to send metrics to Datadog </br> reporters. `reporters.NullReporter` throws the metrics away.</td>  </tbody>  </table> </details>| Initialize more information. All options are optional objects. |
| Gauges          | `metrics.gauge(key, value[, tags[, timestamp]])`|Tags and timestamp are optional.|
| Counters        |`metrics.increment(key[, value[, tags[, timestamp]]])`|The default of value is 1. The Value, tags, and timestamp are optional.|
| Histograms      |`metrics.histogram(key, value[, tags[, timestamp[, options]]])`|                       |
| Distributions   |                                                |                                        |
| Flushing        |                                                |                                        |

## Overview
datadog-metrics is a useful library for sending metrics to Datadog via Datadog’s HTTP API. 

## Benefits
This library is easy to use, one benefit is that you do not need to set up the Datadog Agent to use datadog-metrics. It also solves the problem of slowed down apps performance when using the HTTP API by locally buffering the metrics and regularly sending them to Datadog in batches. By doing so, it ensures that the performance of your app remains unaffected while still providing you with the benefits of utilizing Datadog's powerful metric collection capabilities.

## Setting Up
#### Pre-Requisites
1. Make sure you have Node.js installed. Datadog-metrics requires v12 or later. </br>
2. Gather your <a href="https://docs.datadoghq.com/account_management/api-app-keys/">API key</a>.

#### Install
Install datadog-metrics using npm </br>
```
npm install datadog-metrics --save
```

## Example - Random Metric
Create a javascript file and name it random_metric.js

```js
const metrics = require('datadog-metrics');

function getRandomNumber(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

var randomNumber = getRandomNumber(1, 1000);

metrics.gauge('my_number', randomNumber);
metrics.flush();
```

To run use the command 
```
DATADOG_API_HOST=“<SITE>” DATADOG_API_KEY=“<API_KEY>" DEBUG=metrics node random_metric.js 
```
With your site and API key. 

This will send a metric called 'my_number' to Datadog with a random number from 1 to 1000.

The console output should look like this. In this case 'my_number' has a value of 895.
<img src="/terminal.png" alt="terminal" width="800">

If you would like to see your metric in Datadog navigate to <a href="https://docs.datadoghq.com/metrics/explorer/">metrics explorer</a> and select 'my_number' for your metric. 
<img src="/random.png" alt="random" width="800">
  
## Adding Functionality - Random Metric
```js
const metrics = require('datadog-metrics');

function getRandomNumber(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

metrics.init({ host: 'myhost', prefix: 'myprefix.', defaultTags: ['category:random']});

function sendRandom() {
  var randomNumber = getRandomNumber(1, 1000);
  metrics.gauge('my_number', randomNumber);
}

setInterval(sendRandom, 15000);
```
The number is now being updated every 15 seconds to a new random value.
Use init to provide more more options. In this case we set the host, prefix, and tags. 

The console output should look like this.
<img src="/terminal2.png" alt="terminal" width="800">

Your metric should look like this
<img src="/random2.png" alt="random" width="800">

## Best practices/Format

## Summary

## More
