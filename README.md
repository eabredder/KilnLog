# KilnLog — Quickstart Guide

KilnLog is a single HTML file that runs entirely in your browser. No installation or internet connection required — just open `kilnlog.html` in Chrome, Firefox, or Safari.

**Browser support:** Chrome 80+, Firefox 74+, Safari 13.1+, Edge 80+. Internet Explorer is not supported.

---

## Two Views

The dashboard has two independent tabs, selectable at the top of the page:

| Tab | Data source | What it shows |
|---|---|---|
| **Kilntroller** | Kilntroller CSV export | Precision kiln data from the dual wet/dry-bulb sensor array |
| **SensorPush** | SensorPush CSV export(s) | Ambient sensor data from inside and/or outside the kiln |

The Kilntroller view is the primary reference for kiln schedule compliance. The SensorPush view provides supporting context using a simpler ambient sensor.

---

## Loading Data

Both tabs use the same pattern: drop a file onto the upload area, or click **Browse** to pick one from your computer.

### Kilntroller tab

Drag your kilntroller CSV file anywhere onto the upload area, or click **Browse** inside the drop zone.

Once data is loaded the charts appear and a **Load new file** button appears in the top-right header — click it to reset and load a different file.

### SensorPush tab

The SensorPush tab has two slots — **Inside Kiln** and **Outside** (optional). Drop or click **Browse** in each slot to load a SensorPush CSV export. The outside sensor is not required; if you only have inside data, click **Continue with inside data only** once the inside file is loaded.

Once data is loaded a **Load new files** button appears in the top-right header — click it to reset and load different files.

---

## Filters

Both tabs have two independent filters. Changes to filters in one tab do not affect the other.

### Readings filter

Click the **Readings** card to open the date filter dialog. You can filter by:

- **Date Range** — enter a From and To date/time
- **Month & Year** — select a specific calendar month

Click **Apply** to update the charts, or **Show all** to remove the filter. When a filter is active the Readings card turns highlighted to remind you that you are not viewing the full dataset.

> The **Monthly Averages** chart always shows data across the full dataset regardless of the Readings filter, so long-term seasonal trends remain visible even when you are zoomed into a specific period.

### Time Window filter

The **Time Window** card lets you restrict all charts to a specific part of the day:

- **All** — every reading regardless of time of day (default)
- **Day** — readings within the defined daytime window only
- **Night** — readings outside the daytime window only

When Day or Night is selected, a pair of hour inputs appears so you can adjust the window boundaries. Hours are in UTC; the default daytime window is **6:00–18:00 UTC**. Adjust these to match your local day/night split.

This filter is useful for comparing how the kiln performs during active (daytime) monitoring hours versus overnight.

---

## Showing and Hiding Charts

A collapsible **Charts** bar sits between the filter cards and the chart area. Click it to expand a list of chart names. Click any name to hide that chart; click it again to show it. Use **Show all** to restore all charts at once.

---

## Kilntroller View — Summary Cards

| Card | What it shows |
|---|---|
| **EMC** | Most recent equilibrium moisture content — Wet-bulb (precise, derived from wet/dry-bulb depression) and Estimated (derived from dry-bulb sensor RH alone). The sub-line shows the current firmware fan thresholds as `OFF X.X / ON X.X %`. |
| **Temperature** | Most recent dry-bulb (kiln air temp), wet wick (wet-bulb), and outside temperature in °C. |
| **Humidity** | Most recent kiln relative humidity, outside RH, and the current wet-bulb temperature depression value. |
| **Fan Activity** | Total fan runtime in hours, daily average, and percentage of time fans were on across the loaded dataset. |
| **Readings** | Number of log entries and the date span covered. Click to open the date filter. |
| **Time Window** | All / Day / Night toggle with adjustable hour boundaries (UTC). |
| **Battery** | Most recent battery voltage, estimated state of charge, and voltage range across the dataset. |
| **Solar Panel** | Most recent solar panel voltage and current, and voltage range across the dataset. |

---

## Kilntroller View — Charts

### EMC with fan thresholds
The primary kiln-condition chart. Shows five lines:

- **EMC %** (solid orange) — the primary wet-bulb measurement, derived psychrometrically from the wet/dry-bulb depression
- **Est. EMC** (dashed blue/purple) — a rougher estimate derived from the dry-bulb sensor's ambient RH alone, without the wet wick
- **Fans ON above this** (dashed red) — the upper firmware threshold; fans turn on when EMC rises above this
- **Fans OFF below this** (dashed green) — the lower firmware threshold; fans turn off when EMC drops below this
- **EMC trend** (thin white dashed) — a linear regression trend line over the displayed period

Background shading indicates fan controller state: **orange** = fans on (drying mode), **gray** = fans off. A small note below the chart reports what percentage of readings used the psychrometric (wet-bulb) measurement versus the dry-bulb sensor fallback — if the wick dries out, this proportion drops and the estimate becomes less accurate.

### Fan Activity
The number of fans running (0–4) at each point in time. Useful for spotting patterns in fan cycling and verifying the controller is behaving as expected.

### Temperature Depression
The difference between the dry-bulb and wet-bulb temperatures (°C). This is a direct indicator of wet-bulb wick health. A healthy, well-wetted wick typically shows a depression of **4–8 °C**. If the depression collapses toward zero, the wick likely needs re-wetting. Note: the dry-bulb sensor's anti-condensation heater adds a small upward bias to the absolute values, but the trend is reliable.

### Kiln vs. Outside Temperature
Three lines: **kiln dry-bulb** (solid), **kiln wet-bulb** (dashed), and **outside air temperature**. Yellow shading between the dry-bulb and outside lines indicates periods when the kiln is warmer than outside (solar gain or retained heat); blue shading indicates periods when the kiln is cooler. Absolute dry-bulb readings carry a small upward bias (+0.5–2 °C) from the sensor heater.

### Kiln vs. Outside Humidity
Kiln relative humidity and outside RH plotted together. Useful for seeing how outside conditions are influencing kiln humidity and whether venting is effective.

### Humidity Difference (Kiln − Outside)
The signed difference between kiln and outside RH at each point in time. Positive values mean the kiln is more humid than outside; negative means outside air is more humid than the kiln. A flat line near zero indicates kiln and outside humidity are closely matched.

### Outside Humidity vs. Kiln EMC
Outside RH and kiln EMC on the same chart. Helps identify whether changes in ambient outdoor humidity are driving changes in the kiln's equilibrium moisture content.

### Temperature Difference (Kiln − Outside)
The signed difference between kiln and outside temperature over time. Positive = kiln warmer than outside; negative = kiln cooler.

### Monthly Averages
Month-by-month averages for kiln temperature, outside temperature, temperature difference, kiln RH, outside RH, humidity difference, and EMC. Always reflects the full dataset (not affected by the Readings date filter), but does follow the Time Window filter so you can compare monthly daytime vs. nighttime averages.

### Battery & Solar
Battery voltage and solar panel voltage over time. Useful for confirming the power system is healthy across a long logging period.

### Wick Health
Dedicated chart for monitoring wet-wick condition over time. Shows four traces:

- **Wick RH %** (solid teal, left axis) — the relative humidity measured by the wet-bulb sensor element directly adjacent to the wick. A healthy, well-wetted wick reads consistently above 88 %.
- **Good threshold 88 %** (dashed green) — wick-adjacent RH above this line means the wick is healthy and the psychrometric measurement is trusted at full weight.
- **Failed threshold 70 %** (dashed red) — wick-adjacent RH below this line in dry-air conditions means the firmware has detected a failed wick and switched to sensor-only EMC automatically. Fan control continues uninterrupted.
- **Dew Margin °C** (amber, right axis) — how many degrees above the dew point the kiln air currently is. A large margin means dry air where evaporation is most reliable. When this line drops below ~1.5 °C the ambient air is near-saturated and the psychrometric path is suppressed regardless of wick condition — this is normal behaviour in very humid conditions, not a fault.

A note below the chart counts any readings in the filtered dataset where the wick was flagged as failed or where a physical anomaly was detected.

---

## SensorPush View — Summary Cards

| Card | What it shows |
|---|---|
| **Avg Est. EMC** | Average estimated EMC for the kiln sensor (and outside sensor if loaded) across the filtered dataset. This is an approximation — see note on the Estimated EMC chart below. |
| **Avg Temperature** | Average kiln and outside temperatures in °C across the filtered dataset. |
| **Avg Humidity** | Average kiln and outside relative humidity across the filtered dataset. |
| **Readings** | Number of log entries and date span. Click to open the date filter. |
| **Time Window** | All / Day / Night toggle with adjustable hour boundaries (UTC). |

---

## SensorPush View — Charts

### Estimated EMC
A time-series estimate of equilibrium moisture content derived from the inside kiln sensor's ambient RH and temperature readings. **This is an approximation only** — it uses the Hailwood-Horrobin equation applied to direct RH readings, not a precision psychrometric wet/dry-bulb measurement. Use the Kilntroller view for kiln schedule compliance; treat this chart as supporting context.

### Kiln vs. Outside Humidity
Relative humidity from the inside kiln sensor and outside sensor over time. Only the inside sensor line is shown if no outside file was loaded.

### Humidity Difference (Inside − Outside)
Signed difference between inside and outside RH. Only shown when an outside sensor file is loaded.

### Kiln vs. Outside Temperature
Temperature from the inside and outside sensors over time. Only the inside line is shown if no outside file was loaded.

### Temperature Difference (Inside − Outside)
Signed difference between inside and outside temperature. Only shown when an outside sensor file is loaded.

### Monthly Averages
Month-by-month averages for inside and outside temperature and RH, plus temperature and humidity differences. Like the Kilntroller view, this chart always uses the full dataset and is not affected by the Readings date filter, but it does follow the Time Window filter.

---

## Raw Data Tables

Both views include a scrollable raw data table at the bottom of the page showing every log entry in the currently filtered dataset. This is useful for spot-checking individual readings or exporting a subset of the data.

---

## Tips

- **The Help link** (top-right of every tab) opens this guide and the project documentation on GitHub.
- **Large files load slowly the first time** but filter changes after that are fast — the data is cached in memory until you load a new file.
- **All timestamps in the dashboard are UTC.** If your sensor logs in local time, adjust the Time Window hours accordingly.
- **Watch the EMC mode indicator** below the KT EMC chart. If it reports a low percentage of psychrometric (wet-bulb) readings, open the **Wick Health** chart to see whether the wick-adjacent RH has been dropping below the 88 % good threshold or crossing the 70 % failed threshold — this tells you whether the drop is due to a dry wick or simply near-saturated ambient conditions (which is normal and self-correcting).
- **SensorPush EMC values should not be used for kiln schedule decisions.** They are useful for understanding ambient conditions and long-term seasonal trends, but the kilntroller's wet-bulb measurement is the authoritative source.
- **The Readings card turns highlighted when a date filter is active**, so it is easy to tell at a glance that you are not looking at the full dataset.
- **The delta charts in the SensorPush view** (Humidity Δ and Temperature Δ) only appear when both an inside and outside sensor file have been loaded.
# KilnLog — Quickstart Guide

KilnLog is a single HTML file that runs entirely in your browser. No installation or internet connection required — just open `kilnlog.html` in Chrome, Firefox, or Safari.

**Browser support:** Chrome 80+, Firefox 74+, Safari 13.1+, Edge 80+. Internet Explorer is not supported.

---

## Two Views

The dashboard has two independent tabs, selectable at the top of the page:

| Tab | Data source | What it shows |
|---|---|---|
| **Kilntroller** | Kilntroller CSV export | Precision kiln data from the dual wet/dry-bulb sensor array |
| **SensorPush** | SensorPush CSV export(s) | Ambient sensor data from inside and/or outside the kiln |

The Kilntroller view is the primary reference for kiln schedule compliance. The SensorPush view provides supporting context using a simpler ambient sensor.

---

## Loading Data

### Kilntroller tab

Drag your kilntroller CSV file anywhere onto the page, or click **Load CSV** in the top-right header.

### SensorPush tab

The SensorPush tab has two slots — **Inside Kiln** and **Outside** (optional). Drop or browse a SensorPush CSV export into each slot. The outside sensor is not required; if you only have inside data, click **Continue with inside data only** once the inside file is loaded.

To load new files after the charts are already showing, click **Load new files** in the header.

---

## Filters

Both tabs have two independent filters. Changes to filters in one tab do not affect the other.

### Readings filter

Click the **Readings** card to open the date filter dialog. You can filter by:

- **Date Range** — enter a From and To date/time
- **Month & Year** — select a specific calendar month

Click **Apply** to update the charts, or **Show all** to remove the filter. When a filter is active the Readings card turns highlighted to remind you that you are not viewing the full dataset.

> The **Monthly Averages** chart always shows data across the full dataset regardless of the Readings filter, so long-term seasonal trends remain visible even when you are zoomed into a specific period.

### Time Window filter

The **Time Window** card lets you restrict all charts to a specific part of the day:

- **All** — every reading regardless of time of day (default)
- **Day** — readings within the defined daytime window only
- **Night** — readings outside the daytime window only

When Day or Night is selected, a pair of hour inputs appears so you can adjust the window boundaries. Hours are in UTC; the default daytime window is **6:00–18:00 UTC**. Adjust these to match your local day/night split.

This filter is useful for comparing how the kiln performs during active (daytime) monitoring hours versus overnight.

---

## Showing and Hiding Charts

A collapsible **Charts** bar sits between the filter cards and the chart area. Click it to expand a list of chart names. Click any name to hide that chart; click it again to show it. Use **Show all** to restore all charts at once.

---

## Kilntroller View — Summary Cards

| Card | What it shows |
|---|---|
| **EMC** | Most recent equilibrium moisture content — Wet-bulb (precise, derived from wet/dry-bulb depression) and Estimated (derived from dry-bulb sensor RH alone). The sub-line shows the current firmware fan thresholds as `OFF X.X / ON X.X %`. |
| **Temperature** | Most recent dry-bulb (kiln air temp), wet wick (wet-bulb), and outside temperature in °C. |
| **Humidity** | Most recent kiln relative humidity, outside RH, and the current wet-bulb temperature depression value. |
| **Fan Activity** | Total fan runtime in hours, daily average, and percentage of time fans were on across the loaded dataset. |
| **Readings** | Number of log entries and the date span covered. Click to open the date filter. |
| **Time Window** | All / Day / Night toggle with adjustable hour boundaries (UTC). |
| **Battery** | Most recent battery voltage, estimated state of charge, and voltage range across the dataset. |
| **Solar Panel** | Most recent solar panel voltage and current, and voltage range across the dataset. |

---

## Kilntroller View — Charts

### EMC with fan thresholds
The primary kiln-condition chart. Shows five lines:

- **EMC %** (solid orange) — the primary wet-bulb measurement, derived psychrometrically from the wet/dry-bulb depression
- **Est. EMC** (dashed blue/purple) — a rougher estimate derived from the dry-bulb sensor's ambient RH alone, without the wet wick
- **Fans ON above this** (dashed red) — the upper firmware threshold; fans turn on when EMC rises above this
- **Fans OFF below this** (dashed green) — the lower firmware threshold; fans turn off when EMC drops below this
- **EMC trend** (thin white dashed) — a linear regression trend line over the displayed period

Background shading indicates fan controller state: **orange** = fans on (drying mode), **gray** = fans off. A small note below the chart reports what percentage of readings used the psychrometric (wet-bulb) measurement versus the dry-bulb sensor fallback — if the wick dries out, this proportion drops and the estimate becomes less accurate.

### Fan Activity
The number of fans running (0–4) at each point in time. Useful for spotting patterns in fan cycling and verifying the controller is behaving as expected.

### Temperature Depression
The difference between the dry-bulb and wet-bulb temperatures (°C). This is a direct indicator of wet-bulb wick health. A healthy, well-wetted wick typically shows a depression of **4–8 °C**. If the depression collapses toward zero, the wick likely needs re-wetting. Note: the dry-bulb sensor's anti-condensation heater adds a small upward bias to the absolute values, but the trend is reliable.

### Kiln vs. Outside Temperature
Three lines: **kiln dry-bulb** (solid), **kiln wet-bulb** (dashed), and **outside air temperature**. Yellow shading between the dry-bulb and outside lines indicates periods when the kiln is warmer than outside (solar gain or retained heat); blue shading indicates periods when the kiln is cooler. Absolute dry-bulb readings carry a small upward bias (+0.5–2 °C) from the sensor heater.

### Kiln vs. Outside Humidity
Kiln relative humidity and outside RH plotted together. Useful for seeing how outside conditions are influencing kiln humidity and whether venting is effective.

### Humidity Difference (Kiln − Outside)
The signed difference between kiln and outside RH at each point in time. Positive values mean the kiln is more humid than outside; negative means outside air is more humid than the kiln. A flat line near zero indicates kiln and outside humidity are closely matched.

### Outside Humidity vs. Kiln EMC
Outside RH and kiln EMC on the same chart. Helps identify whether changes in ambient outdoor humidity are driving changes in the kiln's equilibrium moisture content.

### Temperature Difference (Kiln − Outside)
The signed difference between kiln and outside temperature over time. Positive = kiln warmer than outside; negative = kiln cooler.

### Monthly Averages
Month-by-month averages for kiln temperature, outside temperature, temperature difference, kiln RH, outside RH, humidity difference, and EMC. Always reflects the full dataset (not affected by the Readings date filter), but does follow the Time Window filter so you can compare monthly daytime vs. nighttime averages.

### Battery & Solar
Battery voltage and solar panel voltage over time. Useful for confirming the power system is healthy across a long logging period.

### Wick Health
Dedicated chart for monitoring wet-wick condition over time. Shows four traces:

- **Wick RH %** (solid teal, left axis) — the relative humidity measured by the wet-bulb sensor element directly adjacent to the wick. A healthy, well-wetted wick reads consistently above 88 %.
- **Good threshold 88 %** (dashed green) — wick-adjacent RH above this line means the wick is healthy and the psychrometric measurement is trusted at full weight.
- **Failed threshold 70 %** (dashed red) — wick-adjacent RH below this line in dry-air conditions means the firmware has detected a failed wick and switched to sensor-only EMC automatically. Fan control continues uninterrupted.
- **Dew Margin °C** (amber, right axis) — how many degrees above the dew point the kiln air currently is. A large margin means dry air where evaporation is most reliable. When this line drops below ~1.5 °C the ambient air is near-saturated and the psychrometric path is suppressed regardless of wick condition — this is normal behaviour in very humid conditions, not a fault.

A note below the chart counts any readings in the filtered dataset where the wick was flagged as failed or where a physical anomaly was detected.

---

## SensorPush View — Summary Cards

| Card | What it shows |
|---|---|
| **Avg Est. EMC** | Average estimated EMC for the kiln sensor (and outside sensor if loaded) across the filtered dataset. This is an approximation — see note on the Estimated EMC chart below. |
| **Avg Temperature** | Average kiln and outside temperatures in °C across the filtered dataset. |
| **Avg Humidity** | Average kiln and outside relative humidity across the filtered dataset. |
| **Readings** | Number of log entries and date span. Click to open the date filter. |
| **Time Window** | All / Day / Night toggle with adjustable hour boundaries (UTC). |

---

## SensorPush View — Charts

### Estimated EMC
A time-series estimate of equilibrium moisture content derived from the inside kiln sensor's ambient RH and temperature readings. **This is an approximation only** — it uses the Hailwood-Horrobin equation applied to direct RH readings, not a precision psychrometric wet/dry-bulb measurement. Use the Kilntroller view for kiln schedule compliance; treat this chart as supporting context.

### Kiln vs. Outside Humidity
Relative humidity from the inside kiln sensor and outside sensor over time. Only the inside sensor line is shown if no outside file was loaded.

### Humidity Difference (Inside − Outside)
Signed difference between inside and outside RH. Only shown when an outside sensor file is loaded.

### Kiln vs. Outside Temperature
Temperature from the inside and outside sensors over time. Only the inside line is shown if no outside file was loaded.

### Temperature Difference (Inside − Outside)
Signed difference between inside and outside temperature. Only shown when an outside sensor file is loaded.

### Monthly Averages
Month-by-month averages for inside and outside temperature and RH, plus temperature and humidity differences. Like the Kilntroller view, this chart always uses the full dataset and is not affected by the Readings date filter, but it does follow the Time Window filter.

---

## Raw Data Tables

Both views include a scrollable raw data table at the bottom of the page showing every log entry in the currently filtered dataset. This is useful for spot-checking individual readings or exporting a subset of the data.

---

## Tips

- **Large files load slowly the first time** but filter changes after that are fast — the data is cached in memory until you load a new file.
- **All timestamps in the dashboard are UTC.** If your sensor logs in local time, adjust the Time Window hours accordingly.
- **Watch the EMC mode indicator** below the KT EMC chart. If it reports a low percentage of psychrometric (wet-bulb) readings, open the **Wick Health** chart to see whether the wick-adjacent RH has been dropping below the 88 % good threshold or crossing the 70 % failed threshold — this tells you whether the drop is due to a dry wick or simply near-saturated ambient conditions (which is normal and self-correcting).
- **SensorPush EMC values should not be used for kiln schedule decisions.** They are useful for understanding ambient conditions and long-term seasonal trends, but the kilntroller's wet-bulb measurement is the authoritative source.
- **The Readings card turns highlighted when a date filter is active**, so it is easy to tell at a glance that you are not looking at the full dataset.
- **The delta charts in the SensorPush view** (Humidity Δ and Temperature Δ) only appear when both an inside and outside sensor file have been loaded.
