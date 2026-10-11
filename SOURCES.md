# Sources and notes for `us_counties_and_seats.csv`

The file has one row for each of the 3,144 counties and county-equivalents in the Census Bureau's Vintage 2025 estimates. That covers all 50 states and DC: parishes, Alaska boroughs and census areas, Virginia's independent cities, and Connecticut's planning regions. Puerto Rico is not included.

## County population (`county_population`)

| Code | Source | Date of figure | Link |
|---|---|---|---|
| `CENSUS_V2025_COUNTY` | U.S. Census Bureau, Population Estimates Program, Vintage 2025 county totals (`co-est2025-alldata.csv`, column `POPESTIMATE2025`) | July 1, 2025 | https://www2.census.gov/programs-surveys/popest/datasets/2020-2025/counties/totals/co-est2025-alldata.csv |

This is the newest official population figure that exists for every county. The county figures add up to 341,784,857, the national Vintage 2025 total.

## County-seat population (`seat_population`, `seat_2_population`)

Each seat row has its own `*_pop_source` and `*_pop_date` columns.

| Code | Source | Date of figure | Used for | Link |
|---|---|---|---|---|
| `CENSUS_V2025_CITY` | U.S. Census Bureau, Vintage 2025 city and town population estimates (`sub-est2025.csv`, column `POPESTIMATE2025`) | July 1, 2025 | Incorporated cities, towns, villages and boroughs, plus New England/NJ/PA/MI townships (minor civil divisions). 2,990 seats. | https://www2.census.gov/programs-surveys/popest/datasets/2020-2025/cities/totals/sub-est2025.csv |
| `CENSUS_2020_PL` | U.S. Census Bureau, 2020 Census Redistricting Data (P.L. 94-171), table P1 total population | April 1, 2020 | Unincorporated seats that are Census-designated places (CDPs). The Census Bureau publishes no annual estimates for CDPs, so the 2020 count is the newest official number. 102 seats. | https://www2.census.gov/programs-surveys/decennial/2020/data/01-Redistricting_File--PL_94-171/ |
| `CENSUS_V2025_COUNTY` | Same as the county figure | July 1, 2025 | Seats that are coextensive with their county: consolidated city-counties such as San Francisco, Denver, Anchorage and Broomfield, independent cities such as the Virginia cities, Baltimore, St. Louis and Carson City, DC, the five NYC boroughs, and Arlington VA. | (see above) |

`seat_census_name` shows the exact Census record each seat was matched to, so any match can be checked.

## County seats (`county_seat`, `county_seat_2`)

- Main source: Wikidata, property P36 ("capital") on each county item. Only current statements were used: deprecated statements and those with an end date were dropped. Query run Oct 11, 2026. https://query.wikidata.org/
- Check: every seat was compared with the seat field in the infobox of the county's English Wikipedia article (fetched Oct 11, 2026). The few places where they disagreed were reviewed and fixed by hand, for example Tampa, Jacksonville, McRae-Helena, Laurel VA, Cape May Court House NJ, and the second seats of Essex and Plymouth MA and Seneca NY.
- Where a county has two seats, the second is in `county_seat_2` (31 counties, mostly in Arkansas and Mississippi).

## Things to know

- **No seat (21 rows):** Connecticut's 9 planning regions (Connecticut has no county government), Alaska's 11 census areas (unorganized borough), and Kalawao County HI (run by the state Department of Health).
- **Seat with no population figure (6 seats):** East Naples FL, Paulding MS (Jasper Co.'s second seat), Currituck NC, Boston TX (Bowie Co.'s legal seat; the courthouse is in New Boston), Chesterfield Court House VA and Isle of Wight VA. Each is an unincorporated community with no current Census figure.
- **Seat populations are whole-city totals.** Some seats reach into neighboring counties, so in four cases the seat population is larger than the county population: Amarillo (Potter Co.), Sioux Falls (Minnehaha Co.), Dalhart (Dallam Co.) and Pecos (Reeves Co.).
- **Consolidated city-counties** (Nashville, Louisville, Lexington, Macon-Bibb, Butte-Silver Bow, Athens-Clarke, Augusta-Richmond and others) use the Census "balance" or consolidated-government figure. The `notes` column says which one.
- **Date mismatch:** CDP seat populations are 2020 counts, while incorporated seats and all counties are mid-2025 estimates. Compare them with care.

## Seat share of county (`seat_pct_of_county`, `seat_2_pct_of_county`)

`seat_population / county_population * 100`, rounded to 2 decimals. It is blank when a seat has no population figure or the county has no seat. Where the seat's population and the county's come from different dates (the 2020 CDP counts), the ratio mixes years.

## Share of state (`state_population`, `county_pct_of_state`)

`state_population` is the sum of that state's county populations (July 1, 2025). It matches the Census Bureau's published Vintage 2025 state totals exactly for all 51 (50 states plus DC). `county_pct_of_state` is the county's population divided by that total, in percent, to 4 decimals. DC has one county-equivalent, so it is 100%. `us_states_summary.csv` has one row per state with its largest and smallest county shares.
