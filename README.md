# datadog-metrics

## Overview
datadog-metrics is a useful library for sending metrics to Datadog via Datadog’s HTTP API. 

## Benefits
One benefit to using datadog-metrics is that you do not need to set up the Datadog Agent. It also solves the problem of slowed down apps performance when using the HTTP API by __. 

## Setting Up
#### Pre-Requisites
1. Make sure you have Node.js installed. Datadog-metrics requires v12 or later. </br>
2. Gather your <a href="https://docs.datadoghq.com/account_management/api-app-keys/">API key</a>.

#### Install
Install datadog-metrics using npm </br>
```
npm install datadog-metrics --save
```

## Random Metric
Create a javascript file and name it random_metric.js

```
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
  
## Adding On


## Best practices/Format

## Summary

## More
