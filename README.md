# datadog-metrics

## Overview
`datadog-metrics` is a useful library that enables metrics to be sent to Datadog via Datadog’s HTTP API. 

## Benefits
One advantage is that you do not need to set up the Datadog Agent to use `datadog-metrics`. Locally buffering the metrics and sending them to Datadog in batches ensures efficient metric collection. This minimizes the impact on your application's performance while still providing you with the benefits of utilizing Datadog's powerful metric collection capabilities.

## Setting Up
#### Pre-Requisites
1. Make sure you have Node.js installed. `datadog-metrics` requires version 12 or later. </br>
2. Gather your <a href="https://docs.datadoghq.com/account_management/api-app-keys/">API key</a>.

#### Install
Install datadog-metrics using npm </br>
```bash
npm install datadog-metrics --save
```

## Example - Random Metric
Before diving into the availible functionality of `datadog-metrics` let's get started with a quick example. Create a JavaScript file and named random_metric.js and add the following code.

```js
const metrics = require('datadog-metrics');

function getRandomNumber(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

var randomNumber = getRandomNumber(1, 1000);

metrics.gauge('my_number', randomNumber);
metrics.flush();
```

To run use the command with your site and API key. 
```bash
DATADOG_API_HOST="<SITE>" DATADOG_API_KEY="<API_KEY>" DEBUG=metrics node random_metric.js 
```

This will send a metric called `'my_number'` to Datadog with a random number between 1 and 1000.

The console output should look like this. In this case `'my_number'` has a value of 895.
<img src="/terminal.png" alt="terminal" width="800">

If you would like to see your metric in Datadog navigate to <a href="https://docs.datadoghq.com/metrics/explorer/">metrics explorer</a> and select `'my_number'` for your metric. 
<img src="/random.png" alt="random" width="800">

## API Reference
`datadog-metrics` provides the following API methods.

| API             | Syntax                                         | Description                            |
| --------------- | ---------------------------------------------- | -------------------------------------- |
| Initialization  |<details> <summary> `metrics.init(options)` </summary> </br> <table>  <thead>  <tr>  <th>Option</th>  <th>Description</th>  </tr>  </thead>  <tbody>  <tr> <td><code>host</code></td>  <td>Define the host.</td>  <tr> <td><code>prefix</code></td>  <td>Provide a prefix.</td> <tr> <td><code>flushIntervalSeconds</code></td>  <td>How often to send metrics. Default is 15 seconds.</td> <tr> <td><code>site</code></td>  <td>Set site or server. Default is `datadoghq.com`</td> <tr> <td><code>apiKey</code></td>  <td>Can set API key here, but best practice to keep as environment variable.</td> <tr> <td><code>appKey</code></td>  <td>Can set app key here, but best practice to keep as environment variable.</td> <tr> <td><code>defaultTags</code></td>  <td>Sets tags for metrics.</td> <tr> <td><code>onError</code></td>  <td>Handle asynchronous errors sending buffered metrics. Takes optional argument (the error).</td> <tr> <td><code>histogram</code></td>  <td>Set default options for all histograms. Same properties as `histogram()` method.</td> <tr> <td><code>reporter</code></td>  <td>Sends the buffered metrics. Default is `reporters.DatadogReporter` to send metrics to Datadog </br> reporters. `reporters.NullReporter` throws the metrics away.</td>  </tbody>  </table> </details>| Initialize with additional configuration options. Expand to see available options. `options` are optional. |
| Gauges          | `metrics.gauge(key, value[, tags[, timestamp]])`|Record the value of your metric. `tags` and `timestamp` are optional.|
| Counters        |`metrics.increment(key[, value[, tags[, timestamp]]])`|Increase the value of your metric. The default of `value` is 1. `value`, `tags`, and `timestamp` are optional.|
| Histograms      |<details> <summary> `metrics.histogram(key, value[, tags[, timestamp[, options]]])` </summary> </br> <table>  <thead>  <tr>  <th>Option</th>  <th>Value</th>  </tr>  </thead>  <tbody>  <tr> <td><code>aggregates</code></td>  <td>Can be 'max', 'min', 'sum', 'avg', 'median', or 'count'.</td>  <tr> <td><code>percentiles</code></td>  <td>Can be any decimal between 0 and 1.</td></tbody>  </table> </details>|Samples a histogram value. Expand to see available options. `tags`, `timestamp`, and `options` are optional.|
| Distributions   |`metrics.distribution(key, value[, tags[, timestamp]])`|                                        |
| Flushing        |`metrics.flush([onSuccess[, onError]])`|Flush sends metrics when `flushIntervalSeconds` is set to 0. `onSuccess` and `onError` are optional.|

## Adding Functionality - Random Metric
In our random metric example we used `metrics.gauge` to record the value of our metric and `metrics.flush` to ensure the pending metrics are set before the process is terminated.

We can add more functionality, as well.

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
The number is now being updated every 15 seconds to a new random value. We use init to set the host, prefix, and tags. 

The console output should look like this.
<img src="/terminal2.png" alt="terminal" width="800">

Your metric should look like this.
<img src="/random2.png" alt="random" width="800">

## Adding Functionality - Random Metric

## Summary
