# Charts

The Grafite charts package is intended to make it easy to inject (ChartJS) Charts into a Laravel app UI.

[Source Code](https://github.com/grafiteinc/charts)

!!! warning "The charts package only works with ChartJS"

## Setup

```sh
composer require grafite/charts
```

## Create a Chart

The fastest way to get things started is to use the make command for charts.

```
php artisan make:chart {name}
```

The chart will appear in the `App\Charts` namespace.

## Config

You can set a handful of Chart properties in order to get your chart configured well and quickly.

```
title: Chart title (null)
api_url: If you want the chart loaded from an API use this (null)
loader: Whether or not to show the loader animation (true)
loaderColor: Loader animation color (#187bcd)
displayLegend: Display the legend or not (false)
borderWidth: The thickness of the border (2)
displayAxes: Whether or not to show the axes (true)
displayTitle: Whether or not to show the title (true)
height: The chart hieght (100%)
width: The chart width (100%)
barWidth: The width of the bars in bar graphs (40)
type: The chart type (line)
maintainAspectRatio: Whether or not to maintain the aspect ratio (false)
beginAtZero: Begin the chart at zero (true)
axesAttributes: `x-font-size` and `y-font-size` (array) [default is 14]
```

## Labels and Datasets

There are two main methods for the Chart which are `labels` and `datasets`. Each of these mehods need to output data for the chart to have something to generate. You can customize the options of each dataset you add.

Labels must return a `Collection` instance whereas `datasets` must return an array of `Dataset` instances.

## Using a Chart

When using a chart in your app, you can create the class itself then, once its been loaded with labels and datasets. You need to render it.

There are three main methods to use in your `blade` files to generate the charts.

Creates a script link to the ChartJS CDN:
```
$chart->cdn()
```

Adds the canvas tag for the chart to be rendered on:
```
$chart->html()
```

Adds the JavaScript tags containing the chart JS code. It's been minified so its pretty sleek.
```
$chart->script()
```

## Data via an API

If you want to load data from an API endpoint. You can use the following method:

```
$chart->load(route('api_endpoint'))->html();
$chart->script();
```

With this you'll want to have your `api_endpoint` returning something similar to this:

```
$chart = new ExampleChart();
return $chart->apiResponse();
```

## JavaScript Methods available

You can access some JS methods inside your application which can be useful especially when using the Chart loading via APIs.
In any case you can use the following JS methods.

There are two main methods available:

```
{chartId}_refresh
{chartId}_create
```

The `refresh` method is useful for reloading data from an API endpoint. It uses the `fetch` method.
The `create` nethod is run or you on page load.

## JavaScript Events available

Based on the ChartJS click events, there is a CustomEvent emitted to the document to handle these clicks. Simply listen for `grafite-charts-click` and you'll get the items clicked as well as the chart object. Which means you could load events similar to below:

```
document.addEventListener('grafite-charts-click', function (event) {
    event.detail.items[0] // item clicked on, unless multiple are on the same point
    event.detail.chart // chart object
    event.detail.chart.data.labels[event.detail.items[0].index] // is how to access the label that the item has
});
```