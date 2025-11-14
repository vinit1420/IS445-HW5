---
layout: default
title: "Homework 5"
permalink: /homework5/
---

<div class="analysis-buttons" style="margin-bottom: 1.5rem;">
  <a class="btn" href="https://raw.githubusercontent.com/UIUC-iSchool-DataViz/is445_data/main/bfro_reports_fall2022.csv" target="_blank" style="display:inline-block;padding:0.5rem 1rem;margin-right:0.5rem;border-radius:4px;border:1px solid #444;text-decoration:none;">DATA</a>
  <a class="btn" href="https://colab.research.google.com/drive/1WyCQaTWW2_hIhGeSMKQ9ttv8vM_vEwdJ?usp=sharing" target="_blank" style="display:inline-block;padding:0.5rem 1rem;border-radius:4px;border:1px solid #444;text-decoration:none;">ANALYSIS</a>
</div>

The dataset used here comes from the Bigfoot Field Researchers Organization (BFRO) reports and includes information about each reported sighting (date, location, classification) along with weather variables such as temperature, humidity, and wind speed. In this assignment I focused on the mid-temperature at the time of each sighting and how it changes over time.

---

## Plot 1 – Temperature of Sightings Over Time

<img src="/IS445-HW5/assets/hw5_temp_scatter.png" alt="Temperature of Sightings Over Time" style="max-width:100%; height:auto;">

In the first plot, I visualize the relationship between the **year** of each Bigfoot sighting and the **mid-temperature (`temperature_mid`)** at the time of that sighting. Each point in the scatter plot represents a single report: the horizontal position encodes the **year** (temporal variable) and the vertical position encodes the **temperature at the time of the sighting** (quantitative variable). I use a simple scatterplot with a moderate alpha value (`alpha = 0.5`) to reduce overplotting and make dense regions easier to see. The main encoding types here are **position (x)** for time and **position (y)** for temperature; transparency serves as an implicit density cue, with darker areas indicating many overlapping points. On the analysis side, I first converted the original `date` column to a proper datetime type and then created a new `year` column from it so that temporal patterns could be plotted along a continuous axis. This plot provides an overall view of how temperatures at the time of sightings are distributed over the full time span of the dataset and whether sightings tend to cluster around warmer or cooler conditions.

---

## Plot 2 – Yearly Temperature Summary Statistics (Interactive)

<div style="width:100%; max-width:700px; margin: 1rem 0;">
  <iframe
    src="/IS445-HW5/assets/hw5_temp_stats.html"
    width="100%"
    height="500"
    style="border:none;"
  ></iframe>
</div>

For the second visualization, I summarize **yearly temperature statistics** to explore how typical sighting temperatures evolve over time. After deriving the `year` column, I filtered the data to include only reports from **1970 onward**, which removes very early years with very few observations and makes trends easier to interpret. I then grouped the data by year and computed descriptive statistics on `temperature_mid` using `describe()`, which gives metrics such as **count, mean, 25%, 50%, 75%, min,** and **max**. These statistics were reshaped into a long format with columns `year`, `Stat`, and `Value` via `melt`, so that multiple statistics could be plotted as separate series on the same axes. I also replaced zero values with `NaN` so that the chart does not draw misleading lines at zero where there is no real data.

In the Altair chart, the **x-axis encodes `year` as a temporal field** and the **y-axis encodes `Value` as a quantitative field**. I use a **line mark** to show how each statistic changes over time, and **color** is used as a nominal encoding to distinguish the different statistics (`Stat`, such as `"mean"`, `"max"`, `"min"`, `"50%"`, etc.). Putting all the statistics on a shared scale allows the viewer to compare patterns (for example, whether the maximum temperature trend behaves differently from the median). The chart’s wide, relatively short layout emphasizes long-term trends over year-by-year fluctuations.

---

## Interactivity

To make the summary statistics visualization clearer and more engaging, I added an **interactive dropdown selection** in Altair. The dropdown is bound to the `Stat` field through `alt.binding_select`, offering options like `"50%"`, `"mean"`, `"max"`, `"min"`, `"count"`, `"25%"`, and `"75%"`. A corresponding `alt.selection_point` is used to control both **color** and **opacity** conditionally: the selected statistic is shown using its full color and full opacity, while all other lines appear in a light gray with reduced opacity. This interactivity helps the viewer focus on one statistic at a time, reducing visual clutter in a multi-line chart while still preserving the context of the other statistics in the background. Overall, the interactive control makes the visualization more flexible and insightful than a static line chart, especially when exploring how different summaries of temperature (like the median versus the extremes) change over the years.
