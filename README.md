# Predicting Solar Flare Counts with Poisson and Negative Binomial GLMs
A Poisson and negative binomial GLM that predicts how many C-class solar flares an active region produces in 24 hours from its sunspot features, the same count-regression approach used to model insurance claim frequency.

## Overview

This project models how many common (C-class) solar flares an active region of the Sun produces in a 24-hour period, from the physical features of its sunspot group. The flare count is a whole, non-negative number, so this is a count-regression problem, the same family of frequency models used to model insurance claim counts. It is written in Python using pandas and statsmodels, in a single Jupyter notebook.

## The data

The UCI Solar Flare dataset (NOAA, 1978): 1,066 active solar regions, each described by categorical sunspot features such as the modified Zürich class, spot size, spot distribution and previous activity, along with the number of C-, M- and X-class flares produced in the following 24 hours. C-class flares are the target here. Most regions are quiet, with 884 of the 1,066 producing no flares at all.

## The model

Two count models are fit and compared:

* Poisson GLM: the standard starting point for count data, where the variance is assumed equal to the mean.
* Negative binomial GLM: the same model, but it allows the variance to be larger than the mean (overdispersion), which is what the data shows.

The flare count is modelled on a log scale from three predictors (Zürich class, spot distribution and previous activity), so each coefficient acts as a multiplier on the baseline flare rate rather than adding a fixed number of flares. Spot size is left out because it overlaps heavily with the Zürich class, since both come from the same sunspot classification scheme.

## Results

Across the 1,066 regions:

* Overdispersion: the variance (around 0.70) is more than twice the mean (0.30), which is more spread than a Poisson allows.
* Model fit: the negative binomial scores an AIC of 1268 against the Poisson's 1366, a clear improvement.
* Strongest predictor: the Zürich class. Relative to the quiet baseline, the flare rate rises from about 2.5 times for class C up to roughly 8 to 11 times for the largest, most complex classes (E and F).

The negative binomial is the better model because the flare counts vary more than a plain Poisson assumes, and it accounts for that extra spread.

## Why the Zürich class matters

The Zürich class describes the size and magnetic complexity of a sunspot group, and the results line up with the physics: larger, more magnetically complex regions store and release more energy, so they flare more often. The quietest classes sit at or below the baseline rate, while the largest produce several times as many flares.

## Limitations

* The negative binomial's dispersion parameter is fixed at a default rather than estimated from the data.
* The predictors are all categorical and partly overlapping, so spot size is dropped to avoid redundancy with the Zürich class.
* Only C-class flares are modelled. The same approach would extend to the rarer M- and X-class flares.
* The large number of zero-flare regions could be handled explicitly with a zero-inflated model, which separates whether a region flares at all from how many flares it produces.

## Running it

Open the notebook (`GLM project flare.ipynb`) in Jupyter and run the cells in order. Requires Python with pandas, NumPy and statsmodels.
