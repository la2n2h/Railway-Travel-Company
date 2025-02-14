# Railway-Travel-Company
A Startup company that will focus on booking railway travel.

Dataset
Contains valuable country information, including gross domestic product (GDP), the extent of railway infrastructure, ease of doing business score, and more.

Business requirement:
Use this data to make a data-driven recommendation as to which country the business should select to launch its startup.

Success idea:
Consider various factors that can influence the business's success.
Should you prioritize a country with a high GDP or one with a favorable ease of doing business score?
How might the presence of railways and the number of passengers affect the business?
How important is the number of incoming international visitors?
The team has shared the following factors with you, in order of importance:
Current usage of the existing railways.
Total length of existing railways.
GDP per capita.
Ease of conducting business.

# Process
## Clean data
#### Check data type
Ensure all columns have the correct data types (e.g., trip_minutes should be numeric, start_day and stop_day should be dates).
```
SELECT 
    COLUMN_NAME, DATA_TYPE
FROM `deductive-wares-447204-s0.railway.INFORMATION_SCHEMA.COLUMNS`
WHERE TABLE_NAME = 'railway';
```
| COLUMN_NAME               | DATA_TYPE |
|---------------------------|-----------|
| CountryName               | STRING    |
| Region                    | STRING    |
| Continent                 | STRING    |
| Time                      | INT64     |
| GDP                       | STRING    |
| Population                | INT64     |
| EaseOfDoingBusinessScore  | STRING    |
| RailLines                 | STRING    |
| RailwaysPassengersCarried | STRING    |
| NumberOfArrivals          | INT64     |

#### change data type
```
SELECT
CountryName,
Region,
Continent,
Time,
CAST(GDP AS FLOAT64) AS GDP,
Population,
CAST(EaseOfDoingBusinessScore AS FLOAT64) AS EaseOfDoingBusinessScore,
CAST(RailLines AS FLOAT64) AS RailLines,
CAST(RailwaysPassengersCarried AS FLOAT64) AS RailwaysPassengersCarried,
` NumberOfArrivals ` ,
FROM `deductive-wares-447204-s0.railway.railway`;
```

#### Filter Recent Data
Focus on the most recent year's data for accurate analysis.
```
SELECT *
FROM `deductive-wares-447204-s0.railway.railway1`
WHERE Time = (SELECT MAX(Time) FROM `deductive-wares-447204-s0.railway.railway1`);
```
save data into final as railway2024 for data analysis

## Analyze the Data
#### Count the Total tourist in 2024
```
SELECT
sum(NumberOfArrivals)
FROM `deductive-wares-447204-s0.railway.railway2024`
```
| TouristNumber |
|---------------|
| 2246732100    |

#### Count the railway customer carried in 2024
```
SELECT sum(RailwaysPassengersCarried) AS RailwaysPassengersCarried
FROM `deductive-wares-447204-s0.railway.railway2024`
```
| RailwaysPassengersCarried |
|---------------------------|
| 4008820.168               |

#### Tourist by Region
```
SELECT
Region,
sum(NumberOfArrivals) AS TouristNumber
FROM
`deductive-wares-447204-s0.railway.railway2024`
GROUP BY
Region
ORDER BY TouristNumber DESC
```
| Region                             | TouristNumber |
|------------------------------------|---------------|
| Continental Europe                 | 383863500     |
| Eastern & Central Europe           | 366556000     |
| Mediterranean Europe               | 291783000     |
| East Asia                          | 258494000     |
| North America                      | 225832000     |
| Latin America                      | 173718000     |
| South East Asia                    | 147799800     |
| Middle East                        | 90358500      |
| NIS & Russia                       | 86523000      |
| Africa                             | 79029500      |
| Scandinavia                        | 52080000      |
| North Asia                         | 49384000      |
| South Asia                         | 23480000      |
| Australia, New Zealand & Polynesia | 17830800      |

#### Customer carried KM by Region
```
SELECT
Region,
sum(RailwaysPassengersCarried) AS RailwaysPassengersCarried
FROM
`deductive-wares-447204-s0.railway.railway2024`
GROUP BY
Region
ORDER BY RailwaysPassengersCarried DESC
```
 Region                             | RailwaysPassengersCarried |
|------------------------------------|---------------------------|
| East Asia                          | 1439718                   |
| South Asia                         | 1157174                   |
| North Asia                         | 535444                    |
| Continental Europe                 | 365158.4303               |
| NIS & Russia                       | 191410.71                 |
| Mediterranean Europe               | 95216.914                 |
| Eastern & Central Europe           | 65918.124                 |
| South East Asia                    | 34451.05028               |
| North America                      | 34214                     |
| Scandinavia                        | 23256.20723               |
| Australia, New Zealand & Polynesia | 19618.39167               |
| Latin America                      | 19120.36                  |
| Middle East                        | 14875                     |
| Africa                             | 13244.981                 |

#### Railway KM by continental
```
SELECT
Region,
sum(RailLines) AS RailLines
FROM
`deductive-wares-447204-s0.railway.railway2024`
GROUP BY
Region
ORDER BY RailLines DESC
```
| Region                             | RailLines   |
|------------------------------------|-------------|
| North America                      | 197476.1169 |
| NIS & Russia                       | 138015.17   |
| East Asia                          | 104276      |
| Continental Europe                 | 95359.24549 |
| Eastern & Central Europe           | 75111.822   |
| South Asia                         | 71853.07    |
| Mediterranean Europe               | 38901.665   |
| Africa                             | 36127.25    |
| Latin America                      | 21760       |
| Scandinavia                        | 15624       |
| Middle East                        | 9715        |
| South East Asia                    | 9619        |
| North Asia                         | 4109.15     |
| Australia, New Zealand & Polynesia |             |

#### Business score by continental
```
SELECT
Region,
avg(EaseOfDoingBusinessScore) AS EaseOfDoingBusinessScore
FROM
`deductive-wares-447204-s0.railway.railway2024`
GROUP BY
Region
ORDER BY EaseOfDoingBusinessScore DESC
```
| Region                             | EaseOfDoingBusinessScore |
|------------------------------------|--------------------------|
| Scandinavia                        | 81.809456                |
| North Asia                         | 81.00033                 |
| East Asia                          | 76.79075333              |
| Continental Europe                 | 76.054475                |
| NIS & Russia                       | 75.18441778              |
| Eastern & Central Europe           | 75.11359                 |
| Mediterranean Europe               | 71.81185143              |
| Middle East                        | 65.22779727              |
| South East Asia                    | 64.62398545              |
| North America                      | 63.77168636              |
| Latin America                      | 61.28672955              |
| Australia, New Zealand & Polynesia | 60.72027667              |
| South Asia                         | 60.05568                 |
| Africa                             | 56.40228706              |

#### Top 10 country travel by railway
```
SELECT
CountryName,
sum(RailwaysPassengersCarried) AS RailwaysPassengersCarried
FROM
`deductive-wares-447204-s0.railway.railway2024`
GROUP BY
CountryName
ORDER BY RailwaysPassengersCarried DESC
LIMIT 10
```
| CountryName    | RailwaysPassengersCarried |
|----------------|---------------------------|
| China          | 1438606                   |
| India          | 1157174                   |
| Japan          | 435063                    |
| Russia         | 133380.866                |
| France         | 112384                    |
| Germany        | 102026                    |
| Korea          | 100381                    |
| United Kingdom | 82550.43026               |
| Italy          | 56586                     |
| United States  | 32483                     |

#### Correlation between economic indicators with railway travel
BY TABLEAU
#### Top countries has high GDP per capita and ease of doing business score for business set up
BY TABLEAU

## DATA VISUALIZATION
View the interactive Tableau dashboard here: [Raiway travel company Dashboard](https://public.tableau.com/shared/BTTW73NYT?:display_count=n&:origin=viz_share_link)

<div class='tableauPlaceholder' id='viz1739500997688' style='position: relative'><noscript><a href='#'><img alt='SUMMARY ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;BT&#47;BTTW73NYT&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='path' value='shared&#47;BTTW73NYT' /> <param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;BT&#47;BTTW73NYT&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /><param name='ignore_sticky_session' value='yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1739500997688');                    var vizElement = divElement.getElementsByTagName('object')[0];                    if ( divElement.offsetWidth > 800 ) { vizElement.style.width='1000px';vizElement.style.height='3527px';} else if ( divElement.offsetWidth > 500 ) { vizElement.style.width='1000px';vizElement.style.height='3527px';} else { vizElement.style.width='100%';vizElement.style.height='4227px';}                     var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>

