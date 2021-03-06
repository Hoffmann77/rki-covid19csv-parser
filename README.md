# rki-covid19csv-parser
A small python module to work with the RKI_Covid19.csv files issued by the German RKI (Robert Koch Institut) on a daily basis.

## Installation:
```pip install rki-covid19csv-parser```

## Usage:
### First steps:
Initialize the parser and load data from the RKI_Covid19.csv file.   Because of the daily increasing file size this process can take a while.

```
import rki_covid19csv_parser
  
covid_cases = rki_covid19csv_parser.covid_cases()

covid_cases.load_rki_csv('path/to/csv')
```

### Speeding up the loading process:
Once you have loaded the csv file it's possible to save the processed data to a file.
This can speed up the process of loading the data significantly if you whish to run your script more than once.

```
#save file.
covid_cases.save_toFile('desired/path')

#load file.
covid_cases.load_fromFile('path/to/saved/file')
```

### Get the covid19 data:
#### Supported methods:
A description of the parameters can be found below. 

| method | description | returns
| --- | --- | --- |
| `cumCases(date, region_id, date_type)` | cumulated covid19 cases | Filter object |
| `cumDeaths(date, region_id, date_type)` | cumulated covid19 deaths | Filter object |
| `newCases(date, region_id, date_type)` | new covid19 cases | Filter object |
| `newDeaths(date, region_id, date_type)` | new covid19 deaths | Filter object |
| `newCasesTimespan(date, region_id, date_type, timespan)` | new covid19 cases in period | Filter object |
| `newDeathsTimespan(date, region_id, date_type, timespan)` | new covid19 deaths in period | Filter object |
| `activeCases(date, region_id, date_type, days_infectious)` | active covid19 cases | Filter object |
| `sevenDayCaserate(date, region_id, date_type)` | new covid19 cases in 7-days | Filter object |
| `sevenDayIncidence(date, region_id, date_type)` | new covid19 cases per 100k people in 7-days | Filter object |
| `deathRate(date, region_id, days_infectious)` | death rate (activeCases/newDeaths) | Filter object |


#### Parameters:

| parameter | type | description | example |
| --- | :---: | --- | --- |
| `date` | str in  iso-format, datetime.date obj, datetime.datetime obj | The desired date. | '2020-06-01 00:00:00' |
| `region_id` | str | [A list of region-ids can be found here.](https://github.com/Hoffmann77/rki-covid19csv-parser/blob/main/REGION_ID.md) | '0' |
| `date_type` | str | The date type to use. Meldedatum or Refdatum | 'Meldedatum' |
| `timespan` | int | Number of last days to be included in calculation. | 3 |
| `days_infectious` | int | Number of days a case is considered as active. | 14 |

### Get your covid19 data in shape:
Each of the methods mentioned above returns an objct of the class Filter. You can use the following methods to get the data into your desired shape.

| method | description | returns
| --- | --- | --- |
| `values()` | raw data | ndarray |
| `by_cases(raw, decimals)` | absolute number of cases | dict |
| `by_age(frequency, decimals)` | cases sorted into agegroups | dict |
| `by_gender(frequency, decimals)` | cases sorted by gender | dict |
| `by_ageandgener(frequency, decimals)` | cases sorted by age and gender | dict |

| parameter | input type | description | example |
| --- | :---: | --- | --- |
| `frequency` | str | weather you want the absolute or relative number of cases | 'absolute' |
| `decimals` | int | number of decimals | 3 |
| `raw` | bool | True to get raw values | True |


#### Examples:

```
cases = covid_cases.cumCases(date='2021-04-29 00:00:00', region_id='01001', date_type='Meldedatum').by_gender(frequency='absolute')
print(cases)
>>> {'M': 1200, 'W': 1400, 'unbekannt': 130}
```
Example values!

