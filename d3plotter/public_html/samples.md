# Samples
<link rel="stylesheet" href="d3plotter.css">
<script language="JavaScript" src="jquery.min.js"></script>
<script language="JavaScript" src="d3/d3.min.js"></script>
<script language="JavaScript" src="d3plotter.js"></script>

This page contains samples that document the functions implemented in `d3plotter.js` a (higher level) library that provides common commands to plot `.csv` data.

This sample page lists the provided methods together with an example and notes on how to use the function as well as how it is implemented.

Some of the plotters are interactive. Since the plots are generated with JavaScript on the webpage (and not binaries containing the expected result), the interaction works on this page as well.

## Bugs?

Bugs can be reported using the [issues](https://github.com/KommuSoft/d3plotter/issues) page on GitHub.

## Usage

One needs to include the [`d3plotter.css`](d3plotter.css) stylesheet and the [`d3plotter.js`](d3plotter.js) javascript file.

The library can also be used without the programmer having to use *JavaScript* himself, but simply by providing a tag with the `plotter` attribute, like:

    <div plotter="plotBars" dfile="data.csv"></div>

In that case the *JQuery* libarary must be imprted as well. The library will than introduce an `svg` in the `<div>` automatically.

## Functions

Below, the functions are listed with links to the corresponding anchor:

 - [`plotBars`](#plotbars);
 - [`plotGroupedBars`](#plotgroupedbars); and
 - [`plotRows`](#plotrows).

The following variables are implicit: they must be defined before and are assumed to be set before calling the method:

 - `width`: the width of the canvas;
 - `height`: the height of the canvas;
 - `dsid`: the 

The first arguments of every such function are:

 - `svg`: The `<svg>` element to draw content to; and
 - `dfile`: The name of the `.csv` file (including extension) where the data to plot, originates from.

Each *function section* is structured as follows:

 - **data format**: The expected format of the data: do we expect numerical, alphanumerical or another format of data, how should the data be represented (rows/columns)
 - **arguments**: What parameters can be set and how is it done.
 - **sample**: A sample of the expected output for a given data file.
 - **interaction**: a description of the interaction that occurs when a user hovers or clicks on certain elements.

Data is categorized in the following datatypes:

 - `Numeric`: numeric values, these values can be positive or negative can be an integer or a real number;
 - `String`: textual data, for instance names;
 - `Date`: a specific day in a specific month in a specific year.
 - `Time`: a certain moment in a day (specified by hours, minutes and seconds).

## `PlotRows`

This function plots one or more functions with a varying *x*-axis. The data can be plotted on at most two *y*-axes.

### Data format

The `.csv` file is structured as follows:

> Minimum **two** columns. The columns should be named (alphanumerical values on
> the first row of the `.csv` file). The *x* row contains numerical data
> preferably ordered the *y* row(s) should contain numerical data as well.

The function will detect the bounds of the *x* and *y* axes and plot the
accordingly by printing lines between each *(x,y)* tuple. If the *x* rows
are not ordered, the line will move back.

### Arguments

Besides the arguments introduced in the [introduction](#functions), the following
arguments must be given.

 - `xcol`: The name of the column that contains the values for the *x*-axis, if `null` the row number is used.
 - `ycols`: A list of names of the columns to plot with the first *y*-axis, if `null` no graphs are plot with that axis.
 - `y2cols`: A list of names of the columns to plot with the second *y*-axis, if `null` no graphs are plot with that axis.
 - `naxis`: The name of the *x*, *y* and *y2* axis, if `null`, the axes are not named, if length does not match 3, the known axis are labeled.
 - `filled`: If `true`, the plots surface is filled, if `false`, the plots are depicted as lines.

### Sample

**HTML**

```HTML
<div id="sample-plotrows"></div>
```

**JavaScript**

```JavaScript
var margin = {top: 35, right: 100, bottom: 75, left: 100}, width = 800 - margin.left - margin.right, height = 600 - margin.top - margin.bottom;
var rootdiv = d3.select("#sample-plotrows");
var dsid = 1;
var svgr = rootdiv.append("svg").attr("width", width + margin.left + margin.right).attr("height", height + margin.top + margin.bottom);
var svg = svgr.append("g").attr("transform", "translate(" + margin.left + "," + margin.top + ")");
plotRows(svg,"csvfiles/stocks-alter.csv","Month",["AAPL","GOOG","MSFT","IBM"],null,["Time","Stock quote"]);
```

**HTML (alternative)**

```HTML
<div plotter="plotRows" dfile="csvfiles/stocks-alter.csv" xcol="Month" ycols="AAPL,GOOG,MSFT,IBM" naxis="Time,Stock quote"></div>
```

**Output**

<div plotter="plotRows" dfile="csvfiles/stocks-alter.csv" xcol="Month" ycols="AAPL,GOOG,MSFT,IBM" naxis="Time,Stock quote"></div>

### Interaction

 - The legend below the graph is interactive: by clicking on the elements, the lines disappear an reappear on the screen.
 - By default, the lines appear sequentially (specified by the order of `ycols` and `y2cols`). This animation is repeated in the event of reappearance.

## `PlotBars`

## `PlotGroupedBars`

This function plots bars with one variable ranging over the *x*-axis, optionally, the data can be grouped by the `gcol`. The data is plotted using one *y*-axis.

### Data format

The `.csv` file is structured as follows:

> Minimum **two** columns. The columns should be named (alphanumerical values on
> the first row of the `.csv` file). The *x* can contain any number.
> preferably ordered the *y* row(s) should contain numerical data as well.
> Optionally an additional "group" column can be used.

The function will detect the bounds of the *x* and *y* axes and plot the
accordingly by printing lines between each *(x,y)* tuple. If the *x* rows
are not ordered, the line will move back.

### Arguments

Besides the arguments introduced in the [introduction](#functions), the following
arguments must be given.

 - `xcol`: The name of the column that contains the values for the *x*-axis, if `null` the row number is used.
 - `gcol`: The name of the column that contains the values for the *x*-axis, if `null` the row number is used.
 - `ycols`: A list of names of the columns to plot with the first *y*-axis, if `null` no graphs are plot with that axis.
 - `naxis`: The name of the *x*, *y* and *y2* axis, if `null`, the axes are not named, if length does not match 3, the known axis are labeled.

### Sample

**HTML (alternative)**

```HTML
<div plotter="plotGroupedBars" dfile="csvfiles/elections.csv" xcol="Party" gcol="Year" ycols="%" naxis="party,%"></div>
```

**Output**

<div plotter="plotGroupedBars" dfile="csvfiles/elections.csv" xcol="Party" gcol="Year" ycols="%" naxis="party,%"></div>

## Parameters

In `d3plotter`, the name of the parameters also implies how the parameters are parsed as well as the type and semantics. In this file we give a summary.


| Name  | Functions  | Type  | Handler | Comment |
|---|---:|---|---|---|
| `svg`            | `0` | d3 **node** element | none | |
| `dfile`          | `123` | **string** containing filename | `.attr` | |
| `xcol`,`gcol`    | `3` | **string** of csv column | `orDefault(null)`  | `gcol` is used to a a *"groupby"* directive |
| `ycols`,`y2cols` | `123` | **lis**t of strings of the csv columns to be plotted | `splitArguments` |  |
| `naxis`          | `123` | **list** of strings of the axis | `splitArguments` | |
| `filled`         | `1--` | **boolean** determining whether the lines should be filled | `isDefined` | |

List of functions:

| # | Function |
|---|---|
| 0 | &lt;all&gt; |
| 1 | `plotRows` |
| 2 | `plotBars` |
| 3 | `plotGroupedBars` |