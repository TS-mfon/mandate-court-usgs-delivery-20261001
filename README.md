# Recent Significant Earthquake Records (USGS)

A dataset of 50 earthquake events collected from the USGS Earthquake Hazards Program
public catalog, with a resolvable primary source URL for every record.

## Collection date

2026-10-01T11:09:22.717Z

## Source catalog and exact query

All records come from the USGS FDSN event web service. The exact request used was:

```
https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&starttime=2026-09-01&endtime=2026-09-30&minmagnitude=5.0&orderby=magnitude&limit=50
```

Parameters: events between 2026-09-01 and 2026-09-30 (UTC) with a
magnitude of at least 5.0, ordered by magnitude, capped at
50 results. The service returned 50 features;
0 were discarded because at least one required field was null, leaving 50
complete records. No value in this dataset was inferred, interpolated, or substituted.

Service documentation: https://earthquake.usgs.gov/fdsnws/event/1/

## Files

| File | Contents |
| --- | --- |
| `results.json` | 50 earthquake records, one object per event |
| `sources.json` | One entry per `event_id`, giving its USGS event page URL |
| `README.md` | This file |

## Fields in results.json and their units

| Field | Type | Unit / format |
| --- | --- | --- |
| `event_id` | string | USGS event identifier, unique within the catalog |
| `place` | string | Human-readable location as published by USGS |
| `magnitude` | number | Magnitude on the scale given by `magnitude_type`, dimensionless |
| `occurred_at` | string | Event origin time, ISO-8601 in UTC |
| `depth_km` | number | Depth below sea level, kilometres |
| `coordinates.longitude` | number | Decimal degrees, WGS 84, east positive, range -180 to 180 |
| `coordinates.latitude` | number | Decimal degrees, WGS 84, north positive, range -90 to 90 |
| `magnitude_type` | string | Magnitude scale used by USGS for this event, e.g. mww, mb |
| `review_status` | string | USGS review state, `reviewed` or `automatic` |

`magnitude` is dimensionless by definition; it is a logarithmic scale value, not a measurement in
physical units. `depth_km` is positive downward.

## Verifying this dataset

Every record can be re-checked against the authoritative catalog:

```bash
jq -r '.records[0].event_id' results.json
jq -r '.sources[] | select(.event_id == "<event_id>") | .source_url' sources.json
```

Opening that URL shows the USGS event page for the same event. Re-running the query above returns
the same events, though USGS may revise a magnitude or review status after publication, which is why
`retrieved_at` is recorded per source.

## Licence and attribution

USGS earthquake data is in the public domain. Produced for Mandate Court mandate
`MC_d493146cd18d49c1b8881975829e1d6e`.
