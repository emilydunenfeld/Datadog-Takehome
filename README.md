# datadog-metrics
| API             | Syntax                                         | Description                            |
| --------------- | ---------------------------------------------- | -------------------------------------- |
| Initialization  |<table>  <thead>  <tr>  <th>Option</th>  <th>Description</th>  </tr>  </thead>  <tbody>  <tr> <td><code>Host</code></td>  <td>Host Description</td>  <tr> <td><code>Host</code></td>  <td>Host Description</td> <tr> <td><code>Host</code></td>  <td>Host Description</td> <tr> <td><code>Host</code></td>  <td>Host Description</td> <tr> <td><code>Host</code></td>  <td>Host Description</td> <tr> <td><code>Host</code></td>  <td>Host Description</td> <tr> <td><code>Host</code></td>  <td>Host Description</td> <tr> <td><code>Host</code></td>  <td>Host Description</td> <tr> <td><code>Host</code></td>  <td>Host Description</td> <tr> <td><code>Host</code></td>  <td>Host Description</td>  </tbody>  </table>      |                                        |
| Gauges          |                                                |                                        |
| Counters        |                                                |                                        |
| Histograms      |                                                |                                        |
| Distributions   |                                                |                                        |
| Flushing        |                                                |                                        |

## Format

| API             | Syntax                                         | Description                            |
| --------------- | ---------------------------------------------- | -------------------------------------- |
| Initialization  |                                                |                                        |
| Gauges          |                                                |                                        |
| Counters        |                                                |                                        |
| Histograms      |                                                |                                        |
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
