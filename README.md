# Gram Mausam

**Village-level weather advice for farmers.** Block-level forecasts are downscaled to Panchayat level and turned into crop-stage advisories.

Smart India Hackathon 2026 | Problem Statement **26074** | Ministry of Earth Sciences (MoES), India Meteorological Department (IMD) | Theme: Agriculture, FoodTech & Rural Development | Category: Software

**Live demo:** https://panchayat-weather-downscaling-2a6x.vercel.app

---

## The problem

Agro-meteorological advisories rely on block-level forecasts. Rainfall, temperature and humidity can differ sharply between Panchayats in the same block because of terrain, land cover and water bodies. One advisory for a whole block can lead to unnecessary irrigation or spraying in one village and a missed warning in another.

## Our solution

1. **Downscaling engine:** a terrain-aware regression model (elevation and distance to water) infers Panchayat-level rainfall, maximum temperature and humidity from block-level forecasts.
2. **Advisory engine:** rules for each crop and growth stage turn the forecast into plain-language advice, in English and Hindi.
3. **Officer review:** an agro-met officer checks, edits and publishes each advisory before farmers see it.
4. **Farmer view:** a mobile-first "Today" screen with a 5-day strip and clear "what to do" cards.

## Features

- Interactive district map for rainfall, temperature and humidity, by day, at block or Panchayat level
- Panchayat detail with the block forecast alongside for comparison
- Crop-stage advisories for Rice, Maize and Cotton (sowing, vegetative, flowering, harvest)
- Rules for heavy rain, dry spells, heat stress, fungal risk and harvest timing
- Officer console: review, edit and publish
- Accuracy tab comparing the model with the block-copy baseline on held-out days
- English and Hindi
- Responsive layout for phone, tablet, laptop and PC, with light and dark mode

## Important note on data

This prototype runs on **synthetic data** for a pilot district of 3 blocks and 18 Panchayats. The accuracy figures show that the pipeline works. They do not show accuracy on real IMD data. The block forecasts are built in and can be replaced with IMD block forecasts.

## Tech stack

**Prototype (this repo):** HTML, CSS and JavaScript in a single file, with no build step and no dependencies.

**Planned full build:** Python (pandas, xarray, LightGBM), FastAPI, PostgreSQL with PostGIS, React with MapLibre, Docker and Nginx.

## Run it locally

Open `index.html` in any modern browser. No installation or internet connection is needed.

## How the model works

For each variable, the model predicts the difference between the true Panchayat value and the block value, using:
- the Panchayat's elevation relative to its block average,
- its distance to water relative to its block average,
- and how those two interact with the size of the block forecast.

It is fitted in the browser on 300 days of synthetic history and tested on 60 unseen days. Error is measured as RMSE against the baseline that copies the block value to every Panchayat.

## Roadmap

- Load real IMD block forecasts and ERA5 or IMD gridded history
- FastAPI backend with PostGIS storage and an API for other systems
- Gradient-boosted and CNN models with uncertainty ranges
- Extra crops, regional languages, and SMS or IVR delivery
- Feedback from farmers to improve advisories
