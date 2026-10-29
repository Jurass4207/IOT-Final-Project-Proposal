# Data Source Inventory — Taiwan Air Quality Evidence Agent

Status checked: 2026-10-29. This is source discovery, not a completed data quality audit.

| Source | Data / cadence | Fields useful to project | Access / concern | Role |
|---|---|---|---|---|
| [MOENV historical AQI, AQX_P_488](https://data.moenv.gov.tw/dataset/detail/aqx_p_488) | Hourly, historical national station observations; platform lists monthly archives | AQI, pollutant status, SO2, CO, O3, PM10, PM2.5, NO2/NOx/NO, wind, publish time, longitude/latitude, station ID | Historical archive is listed; programmatic download/API route and rate limits still need verification | Primary offline evaluation source |
| [MOENV current AQI, AQX_P_432](https://data.moenv.gov.tw/dataset/detail/aqx_p_432) | Hourly, current station observations | AQI, pollutant status, pollutants, wind, publish time, coordinates, station ID | API examples are documented; confirm key and query limits | Live-data demonstration |
| [MOENV hourly general pollutant observations, AQX_P_15](https://data.moenv.gov.tw/dataset/detail/aqx_p_15) | Hourly records in daily/monthly archive form | Station, pollutant item, units, date, hourly values (00–23) | Wide format needs pivoting; inspect archive availability and field consistency | Historical validation / supplement |
| [MOENV air-pollution sensing IoT overview](https://env.moenv.gov.tw/en/Issue/Index/53b6ae55-bb69-449e-953e-01c2330ca3b8) | Sensor observations described at ~3-minute intervals | PM2.5, temperature, relative humidity and other sensor data | Need to identify actual API endpoint, station mapping, archival policy, sensor quality metadata | Optional finer-resolution extension |
| [Civil IoT Taiwan SensorThings API guide](https://ci.taiwan.gov.tw/dsp/Files/docs/api_guide/%E6%B0%91%E7%94%9F%E5%85%AC%E5%85%B1%E7%89%A9%E8%81%AF%E7%B6%B2%E8%B3%87%E6%96%99%E6%9C%8D%E5%8B%99%E5%B9%B3%E5%8F%B0%E8%88%87SensorThings%20API%E7%B0%A1%E4%BB%8B.pdf) | SensorThings API model | Observations / Things / Datastreams | Endpoint and queries must be verified during data acquisition | Optional integration reference |

## Proposed first data slice

Use the MOENV AQX_P_488 historical dataset for a bounded selection of stations and a continuous period. Inspect:

- historical archive download or API pathway, access credentials, and query limits;
- timestamps and timezone semantics;
- missing/sentinel values and duplicate keys;
- continuity by station and pollutant;
- whether historical API data and monthly downloads can reproduce a common study period;
- ability to map station IDs to coordinates and regions.

Do not freeze a geographic focus until station coverage and data completeness are inspected. Candidate pilot scope: central Taiwan stations or a small multi-region set for comparing local episodes.

## First-pass data model

| Canonical field | Source field(s) | Notes |
|---|---|---|
| station_id | SiteId | Stable key; retain source value |
| station_name | SiteName | Display only, not a join key |
| county | County | Geographic context |
| observed_at | publishtime | Parse explicitly; confirm whether this is observation time or publish time |
| aqi | AQI | Numeric conversion with null handling |
| dominant_pollutant | Pollutant | Categorical field; may be empty |
| pm25, pm10 | PM2.5, PM10 | Preserve source unit |
| so2, co, o3, no2, nox, no | SO2, CO, O3, NO2, NOx, NO | Preserve source units per field |
| wind_speed, wind_direction | WIND_SPEED, WIND_DIREC | May be unavailable for some stations/times |
| latitude, longitude | Latitude, Longitude | Coordinate validation required |
| retrieved_at | client-generated | Needed to assess source freshness and reproducibility |
