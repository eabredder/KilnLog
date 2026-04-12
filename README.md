# KilnLog — Quickstart Guide

KilnLog is a browser-based dashboard for reviewing data logged by the Kilntroller — an automatic fan controller for passive solar lumber drying kilns. Load a CSV (comma-separated values) data file exported from the Kilntroller's SD card and KilnLog displays charts, summary cards, and raw data tables to help you understand how your kiln is performing and whether your drying schedule is on track.

No installation required — just open `kilnlog.html` in Chrome, Firefox, or Safari, or try the live hosted version at **[www.ericbredder.com/KilnLog/kilnlog.html](https://www.ericbredder.com/KilnLog/kilnlog.html)**.

> **Internet connection required on first use.** KilnLog loads its charting library from an external content delivery network (CDN) on startup. Once loaded, the browser caches it for future visits, but the first open — and any session after the browser cache is cleared — needs a connection. Your CSV data never leaves your browser; all processing happens locally on your computer.

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

Drag your Kilntroller CSV file anywhere onto the upload area, or click **Browse** inside the drop zone.

Once data is loaded the charts appear and a **Load new file** button appears in the top-right corner — click it to clear the current data and load a different file.

### SensorPush tab

The SensorPush tab has two slots — **Inside Kiln** and **Outside** (optional). Drop or click **Browse** in each slot to load a SensorPush CSV export. The outside sensor is not required; if you only have inside data, click **Continue with inside data only** once the inside file is loaded.

Once data is loaded a **Load new files** button appears in the top-right corner — click it to clear the current data and load different files.

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

When Day or Night is selected, a pair of hour inputs appears so you can adjust the window boundaries. Hours are in UTC (Coordinated Universal Time — the international time standard used by the Kilntroller for all timestamps). The default daytime window is **6:00–18:00 UTC**. Adjust these to match your local day/night split.

This filter is useful for comparing how the kiln performs during active (daytime) monitoring hours versus overnight.

> **Time Window and EMC thresholds:** The Kilntroller logs separate fan control thresholds for daytime and nighttime operation (the active pair at each log time is recorded in the `EMC_on%` and `EMC_off%` CSV columns). When you filter to **Day**, the EMC chart threshold lines will show the day ON/OFF values; filter to **Night** and they show the night values. When viewing **All** hours the lines step between the two at dawn and dusk, which is the correct behaviour. At the very edges of the day window there may be a row or two where the firmware's solar-based day/night switch and the dashboard's hour-based filter disagree slightly — this is a cosmetic artefact only and does not affect the data.

---

## Showing and Hiding Charts

A collapsible **Charts** bar sits between the filter cards and the chart area. Click it to expand a list of chart names. Click any name to hide that chart; click it again to show it. Use **Show all** to restore all charts at once.

---

## Kilntroller View — Summary Cards

| Card | What it shows |
|---|---|
| **EMC** | Most recent Equilibrium Moisture Content (EMC) — the moisture level that wood will reach if left in the current air conditions indefinitely. Shows both the precise Wet-bulb reading (derived psychrometrically from the wet/dry-bulb temperature depression) and an Estimated reading (derived from the dry-bulb sensor's relative humidity alone). The sub-line shows the Kilntroller's active fan control thresholds as `OFF X.X / ON X.X %`. |
| **Temperature** | Most recent dry-bulb temperature (kiln air temperature), wet-bulb temperature (the cooled sensor with wet wick fitted), and outside air temperature, all in °C. |
| **Humidity** | Most recent kiln relative humidity (RH), outside RH, and the current wet-bulb temperature depression (dry-bulb minus wet-bulb, in °C). |
| **Fan Activity** | Total fan runtime in hours, daily average, and percentage of time fans were running across the loaded dataset. |
| **Readings** | Number of log entries and the date span covered. Click to open the date filter. |
| **Time Window** | All / Day / Night toggle with adjustable hour boundaries (UTC). |
| **Battery** | Most recent battery voltage, estimated state of charge, and voltage range across the dataset. The state of charge percentage comes from the Renogy charge controller, which is calibrated for lead-acid batteries — if you are running a LiFePO4 battery, treat this figure as approximate only. Battery voltage is the reliable signal for LiFePO4 health. |
| **Solar Panel** | Most recent solar panel voltage and current, and voltage range across the dataset. |

---

## Kilntroller View — Charts

### EMC with fan thresholds

The primary kiln-condition chart. Shows five lines:

- **EMC %** (solid orange) — the primary EMC reading, derived psychrometrically from the wet/dry-bulb temperature depression. The psychrometric method uses the Sprung equation to compute vapour pressure from the measured temperature difference, giving a physics-based relative humidity that is independent of the humidity sensor's drift.
- **Est. EMC** (dashed blue/purple) — a secondary estimate derived from the dry-bulb sensor's direct relative humidity reading alone, without the wet wick. Less accurate but available as a fallback.
- **Fans ON above this** (dashed red) — the upper EMC threshold configured on the Kilntroller; fans turn on when EMC rises to or above this value.
- **Fans OFF below this** (dashed green) — the lower EMC threshold; fans turn off when EMC drops to or below this value.
- **EMC trend** (thin white dashed) — a linear regression trend line showing the overall direction of EMC across the displayed period.

Background shading indicates fan controller state: **orange** = fans running (active drying), **gray** = fans off. A small note below the chart reports what percentage of readings used the psychrometric (wet-bulb) method versus the dry-bulb sensor fallback — if the wick dries out, this proportion drops and the estimate becomes less accurate.

### Fan Activity

The number of fans running (0–4) at each point in time. Useful for spotting patterns in fan cycling and verifying the controller is responding to EMC changes as expected.

### Kiln vs. Outside Temperature

Three lines: **kiln dry-bulb** (solid), **kiln wet-bulb** (dashed), and **outside air temperature**. Yellow shading between the dry-bulb and outside lines indicates periods when the kiln is warmer than outside (solar gain or retained heat); blue shading indicates periods when the kiln is cooler. Absolute dry-bulb readings carry a small upward bias (+0.5–2 °C) from the sensor heater.

### Kiln vs. Outside Humidity

Kiln relative humidity and outside relative humidity plotted together. Useful for seeing how outside conditions are influencing kiln humidity and whether venting is effective.

### Humidity Difference (Kiln − Outside)

The signed difference between kiln and outside relative humidity at each point in time. Positive values mean the kiln is more humid than outside; negative values mean outside air is more humid than the kiln. A flat line near zero indicates kiln and outside humidity are closely matched.

### Outside Humidity vs. Kiln EMC

Outside relative humidity and kiln EMC on the same chart. Helps identify whether changes in ambient outdoor humidity are driving changes in the kiln's Equilibrium Moisture Content.

### Temperature Difference (Kiln − Outside)

The signed difference between kiln and outside temperature over time. Positive = kiln warmer than outside; negative = kiln cooler.

### Monthly Averages

Month-by-month averages for kiln temperature, outside temperature, temperature difference, kiln relative humidity, outside relative humidity, humidity difference, and EMC. Always reflects the full dataset (not affected by the Readings date filter), but does follow the Time Window filter so you can compare monthly daytime vs. nighttime averages.

### Battery & Solar

Battery voltage, solar panel voltage, and estimated battery state of charge over time. Useful for confirming the power system is healthy across a long logging period.

> **LiFePO4 note:** The state of charge (Charge %) line is derived from the Renogy charge controller, which uses a voltage-lookup table calibrated for lead-acid batteries. On a LiFePO4 pack the nearly flat voltage-SoC curve means even a small voltage sag under fan load can appear as a large SoC drop — this is a measurement artefact, not real depletion. Use the **battery voltage** line as the reliable health indicator: a healthy 12 V LiFePO4 pack stays between roughly 12.8 V and 13.4 V under normal operating conditions.

### Wick Health

Dedicated chart for monitoring wet-wick condition over time. The wet wick is a water-saturated fabric fitted to the wet-bulb sensor; its continuous evaporation is what produces the temperature depression that makes the psychrometric EMC calculation possible.

The chart combines all three wick diagnostic signals in one view so you can distinguish a dry wick from near-saturated ambient air — two conditions that look similar (collapsed depression) but have different causes and responses.

**Left axis — Wick RH %**

- **Wick RH %** (solid teal) — the relative humidity measured at the wet-bulb sensor surface, directly adjacent to the wick. A healthy, well-wetted wick reads consistently above 88 %.
- **Good threshold 88 %** (dashed green) — wick-adjacent RH above this line means the wick is healthy and the psychrometric measurement is trusted at full weight.
- **Failed threshold 70 %** (dashed red) — wick-adjacent RH below this line in dry-air conditions means the Kilntroller has detected a failed wick and switched to sensor-only EMC automatically. Fan control continues uninterrupted.

**Right axis — °C**

- **Temperature depression dT** (purple, filled) — the difference between the dry-bulb and wet-bulb temperatures. Evaporation from the wet wick cools the wet-bulb sensor below ambient air temperature; this gap is the primary input to the psychrometric calculation. A healthy, well-wetted wick typically shows **4–8 °C** at normal kiln operating temperatures. The dry-bulb sensor's anti-condensation heater adds a small upward bias to the absolute value — watch the trend, not the number.
- **Dew margin °C** (amber) — the difference between the current dry-bulb temperature and the dew point (the temperature at which the air would become fully saturated and condensation would form). A large dew margin indicates dry air where evaporation is most reliable. When this line drops toward zero the air is near-saturated and evaporation is physically suppressed regardless of wick condition.
- **Zero reference** (dashed red) — a baseline to make it easy to see when the depression has collapsed.

**Reading the chart together:** if the depression collapses toward zero, check wick RH and dew margin to identify the cause:

| Depression low | Wick RH | Dew margin | Diagnosis |
|---|---|---|---|
| Yes | Below 70 % | High | Wick is dry — re-wet the wick |
| Yes | Above 88 % | Low (near zero) | Air near-saturated — normal, self-correcting |

A note below the chart counts any readings in the filtered dataset where the wick was flagged as failed or where a physical anomaly was detected.

---

## SensorPush View — Summary Cards

| Card | What it shows |
|---|---|
| **Avg Est. EMC** | Average estimated Equilibrium Moisture Content for the kiln sensor (and outside sensor if loaded) across the filtered dataset. This is an approximation — see the note on the Estimated EMC chart below. |
| **Avg Temperature** | Average kiln and outside temperatures in °C across the filtered dataset. |
| **Avg Humidity** | Average kiln and outside relative humidity across the filtered dataset. |
| **Readings** | Number of log entries and date span. Click to open the date filter. |
| **Time Window** | All / Day / Night toggle with adjustable hour boundaries (UTC). |

---

## SensorPush View — Charts

### Estimated EMC

A time-series estimate of Equilibrium Moisture Content derived from the inside kiln sensor's relative humidity and temperature readings, using the Hailwood-Horrobin equation applied to direct sensor RH. **This is an approximation only** — unlike the Kilntroller's psychrometric method, it does not use a wet-bulb measurement and is subject to the accuracy limits of the humidity sensor. Use the Kilntroller view for kiln schedule compliance; treat this chart as supporting context.

### Kiln vs. Outside Humidity

Relative humidity from the inside kiln sensor and outside sensor over time. Only the inside sensor line is shown if no outside file was loaded.

### Humidity Difference (Inside − Outside)

Signed difference between inside and outside relative humidity. Only shown when an outside sensor file is loaded.

### Kiln vs. Outside Temperature

Temperature from the inside and outside sensors over time. Only the inside line is shown if no outside file was loaded.

### Temperature Difference (Inside − Outside)

Signed difference between inside and outside temperature. Only shown when an outside sensor file is loaded.

### Monthly Averages

Month-by-month averages for inside and outside temperature and relative humidity, plus temperature and humidity differences. Like the Kilntroller view, this chart always uses the full dataset and is not affected by the Readings date filter, but it does follow the Time Window filter.

---

## Raw Data Tables

Both views include a scrollable raw data table at the bottom of the page showing every log entry in the currently filtered dataset. This is useful for spot-checking individual readings or reviewing the data in detail.

---

## Tips

- **The Help link** (top-right corner of every tab) opens this guide in a new tab — no GitHub account needed.
- **Large files load slowly the first time** but filter changes after that are fast — the data is held in memory until you load a new file.
- **All timestamps are in UTC** (Coordinated Universal Time). If your sensor logs in local time, adjust the Time Window hours accordingly so the Day and Night filters align with your actual daylight hours.
- **Watch the EMC mode indicator** below the Kilntroller EMC chart. If it reports a low percentage of psychrometric (wet-bulb) readings, open the **Wick Health** chart to see whether the wick-adjacent RH has been dropping below the 88 % good threshold or crossing the 70 % failed threshold — this tells you whether the drop is due to a dry wick that needs attention, or simply near-saturated ambient conditions (which is normal and self-correcting).
- **SensorPush EMC values should not be used for kiln schedule decisions.** They are useful for understanding ambient conditions and long-term seasonal trends, but the Kilntroller's psychrometric wet-bulb measurement is the authoritative source.
- **The Readings card turns highlighted when a date filter is active**, so it is easy to tell at a glance that you are not viewing the full dataset.
- **The humidity and temperature difference charts in the SensorPush view** only appear when both an inside and outside sensor file have been loaded.
