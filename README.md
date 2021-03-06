# ledger-reports
Generate informational graphs from ledger-cli accounting files.

## reportgraph.py

Generate a directed graph (in GraphViz markup) of how money moves within
the file.

May not work with multiple commodities, fixes are welcome.

## report

Creates multiple interesting graphs from the data via [gnuplot](http://www.gnuplot.info/)
and [piechart](https://github.com/cbdevnet/piechart/).

Designed to be easily extendable (by editing it) with queries interesting to yourself.

### Example outputs

![A pie chart, showing asset split](https://raw.githubusercontent.com/cbdevnet/ledger-reports/assets/assets/AssetsOverview-Pie.svg?sanitize=true)

![A line chart, showing monthly expenses](https://raw.githubusercontent.com/cbdevnet/ledger-reports/assets/assets/Expenses-Monthly.svg?sanitize=true)

![A bar chart, showing monthly food expenses](https://raw.githubusercontent.com/cbdevnet/ledger-reports/assets/assets/Food-Monthly.svg?sanitize=true)

### Requirements

* At least ledger 3
* gnuplot
* piechart
* The files `ledgerrc` and `script.gnuplot` from this repository

### Usage

Create a directory `reports/`.

Run as `./report <ledger file> [periods <period expressions>]`

If no ledger file is specified, `main.ledger` is assumed.

The resulting graphs will be placed in the reports/ directory.

The script uses the following categories in the current state
* `Assets`
* `Assets:Liquid`
* `Expenses`
* `Expenses:Food`
* `Liabilities`
* `Income`

If your ledger file uses a different layout or you want to create new
graphs, you will have to edit the `report` script. The drawing tool invocations
are abstracted away to a large degree via bash functions.

Note that the `ledgerrc` file currently contains the *strict* option,
which requires that all accounts and commodities be pre-announced.
If you don't want that, remove the option from the file.

### The `periodic` feature

The periodic feature is experimental and might not work exactly as expected.

Reporting periods must be exactly equal-width buckets, meaning
that the query result must have the exact same number of lines.

This also includes things such as dates without a transaction (a problem
which may be sidestepped by using `--empty`) and leap years,
making `--daily` probably a bad window. `--weekly --empty` is probably a good
minimum interval.

The periodic report will create the same type of graphs from
the same queries as the regular report, but will try to
plot the data for every period as its own line.

Data which can not reliably be segmented into equal-width
periods should be plotted with the `\_aperiodic` functions,
which always produce the same plot.

### Example invocations

`./report`

`./report whatever.ledger`

`./report main.ledger periods 2015 2016`
