# Documentation

The purpose of this website is to allow easy access and analysis of 
Covid-19 data for the state of Texas, including subsets by major metropolitan areas, and by individual counties. The underlying data is updated every day at 1:00 PM.

## Data

The daily data (cases by county, state totals, and Total tests) is pulled off
the website for the Texas State Department of Health 
[https://dshs.texas.gov/news/updates.shtm#coronavirus](https://dshs.texas.gov/news/updates.shtm#coronavirus).

There are a few peculiarities of this data which users should be aware of:

1. The data reported is what they had received at 8 PM the previous day
2. They update their website by noon
3. They have a category "Pending County Assignment" that can be quite large. The county assigned to a case is the county of residence, **not** the reporting county.

For these reasons, my numbers will differ from news accounts, and tend to lag by about a day. County and metro area total will lag by several days as the state researches county of residence, but the state total should be fairly current.

On March 25, the following message appeared on the state website:

_Why did the case counts reported by DSHS increase suddenly?_

_DSHS updated the method of reporting COVID-19 cases in Texas to provide the public with more timely information. The DSHS daily case count now includes all cases reported publicly by local health departments around the state. With the change, Texas is now reporting an additional 305 cases of COVID-19._

## Graph

The Graph tab is designed to allow easy data analysis to understand the time progression of the epidemic.

Epidemics grow exponentially, or not at all. The basic equation governing epidemic growth is
$$N_{d+1}=(1+Ep)N_{d}$$ where $N_{d} =$ number of people infected on day d,
$E =$ number of people an infected person exposes every day, and $p =$ the
probability that the exposure becomes an infection.

Counter measures include "social distancing", which reduces E, and hygiene
measures like hand washing, which reduces p. 

### Choose the data

Data may be selected by Region (metro area or the whole state) or by individual county.

The metro areas follow the standard definition, and consist of counties:

*  Houston: Harris, Fort Bend, Galveston, Waller, Montgomery, Liberty, Brazoria, Chambers, and Austin
*  Dallas: Collin, Dallas, Denton, Ellis, Hood, Hunt, Johnson, Kaufman, Parker, Rockwall, Somervell, Tarrant and Wise
*  San Antonio: Atascosa, Bandera, Bexar, Comal, Guadalupe, Kendall, Medina, and Wilson
*  Austin: Bastrop, Caldwell, Hays, Travis, and Williamson

### Plotting options

Under Plotting Options are several toggles that will add things to the plot, or adjust the appearance of the plot.

#### Crowd sizes to avoid

Suppose I want there to be a less than 1% change of interacting with someone
who is infected. It is simple to calculate, it is just the number of infected
people, divided by the total population in question, times 0.01. This can
give surprisingly small numbers rather quickly, and highlights how crucial 
it is to not allow crowds of people to gather.

#### Expand scale

By default, I chop off the Y scale so that the actual data is easily seen.
The toogle will expand (or sometimes reduce) the scale so that all 
plotted objects are visible.

#### Est Missed cases

Trying to get a handle on how many cases have been missed due to lack of testing, 
I assume the global average fit (see below) and force that to fit at the latest 
data point. I should probably allow the user to input an estimate of current
missed cases and fit to that.

#### Log Scaling

The actual fit is done to the logarithm of the number of cases (as can be
seen in the displayed fit equation), because that makes the exponential a 
linear function. A key aspect of the epidemic will be to see a change in the
slope of the fit, indicating that the growth rate is slowing. That will be
easier to see on a Log plot.

### Data Fits

There are a variety of options to try to fit the data and thus predict
the future of the epidemic.

#### Fit Data

This option does a linear regression on $log(N) = md + b$ to find $m$ 
and $b$, the slope and intercept. The error bars derive from that fit
and represent one standard deviation. Additionally, at the bottom of the page, 
the $R^{2}$ value also derives from that fit.

By default, I weight the fitting, to bias the fit towards the newer data,
assuming that because of the slow ramp-up of testing, the early numbers are
probably not very good. To weight them, I arbitrarily use a function
$w = d^{1.5}$, which should represent $\frac{1}{\sigma^{2}}$. In practice, 
this rather arbitrary weighting seems to help the fit approach the global
average fit. Note that you can turn off the weighting if you wish.

#### Worldwide

The growth rate is amazingly consistent worldwide. The number I am using
comes from [dart throwing chimp](https://dartthrowingchimp.shinyapps.io/covid19-app/), and it is 
easy to see on his growth graph that most countries have followed similar 
trends. His 0.3 growth value translates to my 0.13 slope since I use log 
base 10, and he uses natural logarithms.

Because the global value is only a slope, I derive the y intercept by 
tieing the curve to the most recent datapoint.

#### User Entry

You can play with values on your own. One thing to keep an eye on as
you play with the fit, is what happens to the doubling time.

The doubling time depends only on the slope of the fit, and tells you how many
days it will take for the number of cases to double. Usually a pretty 
frightening number.

#### Multiply cases

In yet another attempt to get a handle on the effect of undetected
infectious people wandering around, you can create a new curve which is the
original fit, but where the number of cases is multiplied by a constant.
Did the lack of testing miss half the cases? Multiply by 2.

## Map

The map is pretty simple. You can color counties by total reported cases,
or by cases per 100,000 population. Note that the "Pending County Assignment"
problem means that there can be a significant lag in a case being tied to
a county.

