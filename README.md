# U.S. Refugee Data and Analysis

This fork contains qualitative analysis of the data and analysis supporting the BuzzFeed News article, "[Where U.S. Refugees Come From — And Go — In Charts](http://www.buzzfeed.com/jsvine/where-us-refugees-come-from-and-go-in-charts)," published on November 19, 2015.

## Data wrangling errors identified

In `scripts/clean-wraps-destination.py`, an entire record of refugees from Zimbabwe to Dallas, Texas, was inadvertently omitted from this analysis. The sugment below appears to be a way of finding where the header rows end and data beigins. This code gets `arrivals` records that include "From :" in the `from_date` column. The `split_point` variable return the index of the first item that matched that boolean filter, which was zero in this case.

```
split_point = _arrivals[
    ~_arrivals["from_date"].str.contains("From: ")
].index[0]
```

Therefore, the `arrivals` table is missing the last row in `_arrivals`, due to this line.

```
arrivals = _arrivals.ix[:split_point - 1].copy()
```

That line contained numbers of refugees from Zimbabwe to Dallas.
```
From: 01 Jan 2005,To: 31 Dec 2015,Zimbabwe,CY 2015,5,Texas,0,Dallas,0,"60,640"
```

## Data

The refugee data comes from the [Department of State's Refugee Processing Center](https://www.wrapsnet.org/)'s [data portal](http://www.wrapsnet.org/Reports/InteractiveReporting/tabid/393/Default.aspx). Specifically, it comes from the "Arrivals by Destination and Nationality" and "Arrivals by Nationality and Religion" reports, and was generated on November 18, 2015. The raw and cleaned files can be found in this repository's [`data` directory](data/).

To calculate per-capita arrivals by state, the analysis uses the [Census Bureau's 2014 population estimates](http://www.census.gov/popest/data/state/asrh/2014/index.html).

## Analysis and Charts

The code behind the analysis and charts was written in Python, and [can be found here](notebooks/us-refugee-analysis.ipynb).

To re-run the analysis yourself, you'll need to install the Python requirements listed in [`requirements.txt`](requirements.txt).

Alternatively, you can [run the analysis in your browser via Binder](http://mybinder.org/repo/BuzzFeedNews/2015-11-refugees-in-the-united-states) — no installation required.


## Questions / Feedback?

Email the author, Jeremy Singer-Vine, at jeremy.singer-vine@buzzfeed.com.
