---
name: UFO Sightings in the US
tools: [Python, Altair, vega-lite]
image: assets/pngs/ufo_heatmap.png
description: HW5 for IS 445. Two interactive Altair charts that look at when Americans report UFO sightings and what shapes they describe.
custom_js:
  - vega.min
  - vega-lite.min
  - vega-embed.min
  - justcharts
---

# UFO Sightings in the United States

For this project I used the National UFO Reporting Center (NUFORC) dataset from class, which has about 80,000 UFO reports with a date, location, reported shape, and duration. I focused on the roughly 57,000 sightings reported in the United States between 1995 and 2013 and asked two simple questions: how has the number of sightings changed over time, and when during the year and the day do people actually see them?

## Plot 1: Sightings per year by reported shape

<vegachart schema-url="{{ site.baseurl }}/assets/json/ufo_sightings_by_year.json" style="width: 100%"></vegachart>

This line chart shows how many UFO sightings were reported in the US each year, with one line for each of the six most common shapes (light, triangle, circle, fireball, sphere, and disk) and one line for everything else. For encodings, the year is on the x axis as an ordinal variable since the data is counted in whole years, the number of sightings is on the y axis as a quantitative variable, and the shape is mapped to color as a nominal variable. Because shape has no natural order, I used a categorical color scheme based on tableau10 so each shape gets a clearly different hue, and I colored the "other" group gray so it reads as background instead of competing with the named shapes. On the analysis side, I converted the date strings into timestamps with pandas (fixing a few times written as 24:00 first), kept only US reports, and limited the years to 1995 through 2013 because there are very few reports before NUFORC's online form and 2014 only covers January to early May. I also grouped about 30 shape labels into the top six plus "other", and counted sightings per year and shape in pandas so the saved chart only holds about 130 rows instead of the full dataset. The chart shows that reports grew steadily and peaked in 2012, and that "light" is by far the most common description, while fireball reports jumped sharply after 2010.

## Plot 2: When do people see UFOs?

<vegachart schema-url="{{ site.baseurl }}/assets/json/ufo_sightings_heatmap.json" style="width: 100%"></vegachart>

This heatmap shows the number of US sightings for every combination of month and hour of the day. The hour is on the x axis and the month is on the y axis, both as ordinal variables, and the number of sightings is encoded as the color of each cell, which is a quantitative variable. Since the count goes from low to high, I used the sequential viridis colormap, where brighter yellow means more sightings. I chose viridis because it is perceptually uniform, so equal changes in count look like equal changes in color, and it stays readable for people with common types of color blindness. For the data transformations, I extracted the month and hour from the cleaned timestamps, grouped the shapes the same way as in Plot 1, and counted sightings for each month, hour, and shape combination in pandas so the dropdown can filter by shape without loading all 57,000 rows into the page. The pattern is very clear: about half of all sightings happen between 8pm and midnight, and the brightest cell is July at 10pm, which lines up with July 4th being the single busiest day in the dataset (about 1,000 reports). That suggests many "UFOs" are probably fireworks, lanterns, or other things people notice when they are outside on summer nights.

## Interactivity

Both charts are interactive beyond the built in pan and zoom. In Plot 1, the legend is clickable: clicking a shape highlights its line and fades all the others (shift click selects more than one). With seven lines that cross each other, it is hard to follow a single shape, so this lets the reader isolate one trend, such as the jump in fireball reports after 2010, without losing the others for comparison. In Plot 2, a dropdown menu below the chart filters the heatmap to a single reported shape. This makes it easy to check whether the summer night pattern holds for every shape or is driven by one type; for example, selecting "fireball" makes the July 4th hotspot even stronger, which supports the fireworks explanation. Both charts also have tooltips, so hovering over a point or cell shows the exact numbers.

## The Data & The Analysis

<!-- these are written in a combo of html and liquid -->

<div class="left">
{% include elements/button.html link="https://raw.githubusercontent.com/UIUC-iSchool-DataViz/is445_data/main/ufo-scrubbed-geocoded-time-standardized-00.csv" text="The Data" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/oliverlee0417/oliverlee0417.github.io/blob/main/python_notebooks/hw5_ufo_sightings.ipynb" text="The Analysis" %}
</div>
