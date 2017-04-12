# Reporting API for Publishers v.1.0.3

### v1.0.3
> Add “region” request parameter. if use “region”  statistics info will break down by country。
> usage:
> region=all
> media statistics info return as a json list，each object contain a certain country info：
> request example： get http://dvx.domob.cn/api/applications/XXXXXXX
> result example：[{"dt":"20170401","request":"1081428","imp_start":"420","imp_finish":"315","clk":"83","revenue":11912120,"ecpm":37.82}, “region” :11560000, {"dt":"20170401","request":"1081428","imp_start":"420","imp_finish":"315","clk":"83","revenue":11912120,"ecpm”:37.82，region” :11560000,}…]
> region=SPECIFIC_REGION_CODE
> you can get specific country statistics by this method. regoin code map you could find in Region Code section.

### v1.0.2
    Add USD($) currency type for reporting. you can add a "cur=usd" query param to get reporting currency type as USD, "cur" param is optional, default currency type is RMB. (using bid exchange rate from Yahoo! and updated by day, this number should be only used for reference, actual exchange rate may different.)

    Add v2 version for reporting(revenue unit change to *1 rather than v1's *1000000) to use v2 version , query URL changed from /api/applications to /api_v2/applications.
    
### v1.0.1
    add hr section support for specify time info.

#### 1.
| Resource |  Param(s) | Description |
| ---------- | -----------: | :------------: |
|GET /api/applications |key=[API TOKEN]|	return all apps info|

```php
[{  "deverid": "62555", //Domob developer id
    "pubid" : "XXXXX", //publisher id  of the app
    "name" : "割绳子", //name of the app
    "platform" : "iOS", //platform
    "status" : "1" //app status，0 for running,  1 for paused
},
… …
]
```
#### 2.
| Resource |  Param(s) | Description |
| ---------- | -----------: | :------------: |
| |key=[API TOKEN] ||
||dt=[yyyy-mm-dd]||
||or||
||key=[API TOKEN]||
||dt=[yyyy-mm-dd]||
|GET /api/applications/[PUBLISHER ID]|hr=[hh]|return statistic data for specific date of the app|
||or||
||key=[API TOKEN]||
||dt_start=[yyyy-mm-dd]||
||dt_end=[yyyy-mm-dd]||
||[cur=usd]||

##### Example
```php
{
    "date" : “2012-08-16",  //date of the data
    "request" : 7275,  //numbers of user's requests
    "imp_start" : 7201, //video start
    "imp_finish" : 7102, //video finish
    "eCPM" : 40.636, //eCPM in RMB
    "revenue" : 288,600,000,  //revenue*1000000 in RMB
    ......
}

{
    "date" : "012-08-16",  //date of the data
    "hr" : "12" , //time of the data
    "request" : 7275,  //numbers of user’s requests
    "imp_start" : 7201, //video start
    "imp_finish" : 7102, //video finish
    "eCPM": 40.636, //eCPM in RMB
    "revenue" : 288,600,000,  //revenue*1000000 in RMB
    ......
}

{
    "dt_start" : "2012-08-16",  //start date
    "dt_end": "2012-9-16",   //end date
    "request" : 14065,   //numbers of user’s requests
    "imp_start" : 13950,  //video start
    "imp_finish" : 13901, //video finish
    "eCPM": 43.062, //eCPM in RMB
    "revenue" : 598,600,000,  //revenue*1000000 in RMB
    ......
}
```

#### 3.
    1 . Get all applications.
``` php
 bash: curl -X GET \
        -G 'http://dvx.domob.cn/api/applications' \
        -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm'

 return:
[
    {
        "deverid": "5...9",
        "pubid": "96Z...-edY/wTBSK",
        "name": "XXXX Run",
        "platform": 2,
        "status": 0
    },
    {
        "deverid": "5...9",
        "pubid": "96Z...zedY/wTPrJ",
        "name": "XXX Legend",
        "platform": 2,
        "status": 0
    },
    {
        "deverid": "5...9",
        "pubid": "96ZJ...edY/wTPrI",
        "name": "XXX Farm",
        "platform": 2,
        "status": 0
    },
    {
        "deverid": "5...9",
        "pubid": "96ZJ...edY/wTPrL",
        "name": "XXX 3D",
        "platform": 2,
        "status": 0
    }
]
```

    2 . Get specific date statistics of one application.

```php
 bash: curl -X GET \
            -G 'http://dvx.domob.cn/api/applications/96Z...-edY/wTBSK' \
            -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm' \
            -d 'dt=20160504'

return:
{
    "dt": "20160504",
    "request": "7504",
    "imp_start": "5835",
    "imp_finish": "4413",
    "clk": "140",
    "revenue": "264780000",
    "ecpm": 60
}
```
    3 . Get specific date range statistics of one application.
```php
bash:curl -X GET \
          G 'http://dvx.domob.cn/api/applications/96Z...-edY/wTBSK' \
          -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm' \
          -d 'dt_start=20160504' \
          -d 'dt_end=20160604'

return:
{
    "dt_start": "20160504",
    "dt_end": "20160604",
    "request": "258869",
    "imp_start": "201774",
    "imp_finish": "152308",
    "clk": "7856",
    "revenue": "11115880000",
    "ecpm": 111.69
}
```
    4 . Get specific date statistics of one application.
```php
bash: curl -X GET \
           -G 'http://dvx.domob.cn/api/applications/96Z...-edY/wTBSK' \
           -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm' \
           -d 'dt=20160504' \
           -d 'hr=12'

return:
{
    "dt": "20160504",
    "hr": "12",
    "request": "7504",
    "imp_start": "5835",
    "imp_finish": "1213",
    "clk": "56",
    "revenue": "264780000",
    "ecpm": 72
}
```
    5 . Using "cur=usd" when get report.
```php
bash: curl -X GET \
           -G 'http://dvx.domob.cn/api/applications/96Z...-edY/wTBSK' \
           -d 'key=NTA0Mzk4MTgwNzA2..2MjNhZWNiNzRm' \
           -d 'dt=20160504' \
           -d 'hr=12' \
           -d 'cur=usd'

return:
{
    "dt": "20160504",
    "hr": "12",
    "request": "7504",
    "imp_start": "5835",
    "imp_finish": "1213",
    "clk": "56",
    "revenue": "38472534",
    "ecpm": 10.46
}
```


##### Enclosure1
| code | region |
| -------- | -----: |
|71000000|Taiwan|
|81000000|Hong Kong|
|82000000|Macao|
|1020000000|Andorra|
|1784000000|The United Arab Emirates|
|1004000000|Afghanistan|
|1028000000|Antigua and Barbuda|
|1660000000|Anguilla|
|1008000000|Albania|
|1051000000|Armenia|
|1024000000|Angola|
|1010000000|Antarctica|
|1032000000|Argentina|
|1016000000|American Samoa|
|1040000000|Austria|
|1036000000|Australia|
|1533000000|Aruba|
|1248000000|Oran Islands|
|1031000000|Azerbaijan|
|1070000000|Bosnia and Herzegovina|
|1052000000|Barbados|
|1050000000|Bengal|
|1056000000|Belgium|
|1854000000|burkina faso|
|1100000000|Bulgaria|
|1048000000|Bahrain|
|1108000000|burundi|
|1204000000|Benin|
|1652000000|Saint-Barthelemy|
|1060000000|Bermuda|
|1096000000|Brunei|
|1068000000|bolivia|
|1535000000|Caribbean Netherlands|
|1076000000|Brazil|
|1044000000|Bahamas|
|1064000000|Bhutan|
|1074000000|Bouvet Island|
|1072000000|botswana|
|1112000000|Belarus|
|1084000000|Belize|
|1124000000|Canada|
|1166000000|Cocos Islands|
|1180000000|Congo (DRC)|
|1140000000|Central African|
|1178000000|Congo (cloth)|
|1756000000|Switzerland|
|1384000000|Cote d'Ivoire|
|1184000000|Cook Islands|
|1152000000|Chile|
|1120000000|Cameroon|
|1156000000|China|
|1170000000|Columbia|
|1188000000|Costa Rica|
|1192000000|Cuba|
|1132000000|Cape Verde|
|1162000000|Christmas Island|
|1196000000|Cyprus|
|1203000000|Czech|
|1276000000|Germany|
|1262000000|Djibouti|
|1208000000|Denmark|
|1212000000|Dominica|
|1214000000|Dominican|
|1012000000|Algeria|
|1218000000|Ecuador|
|1233000000|Estonia|
|1818000000|Egypt|
|1732000000|Western Sahara|
|1232000000|Eritrea|
|1724000000|Spain|
|1231000000|Ethiopia|
|1246000000|Finland|
|1242000000|Fiji Islands|
|1238000000|Malvinas Islands (Falkland Islands)|
|1583000000|Micronesia|
|1234000000|Faroe Islands|
|1250000000|France|
|1266000000|Gabon|
|1826000000|Britain|
|1308000000|Grenada|
|1268000000|Georgia|
|1254000000|French Guiana|
|1831000000|Guernsey|
|1288000000|Ghana|
|1292000000|Gibraltar|
|1304000000|Greenland|
|1270000000|Gambia|
|1324000000|Guinea|
|1312000000|Gua de Ropp|
|1226000000|Equatorial Guinea|
|1300000000|Greece|
|1239000000|South Georgia and the South Sandwich Islands|
|1320000000|Guatemala|
|1316000000|Guam|
|1624000000|Guinea-Bissau|
|1328000000|Guyana|
|1334000000|Hird and Macdonald Islands|
|1340000000|Honduras|
|1191000000|Croatia|
|1332000000|Haiti|
|1348000000|Hungary|
|1360000000|Indonesia|
|1372000000|Ireland|
|1376000000|Israel|
|1833000000|The Isle of man.|
|1356000000|India|
|1086000000|British India Ocean Territory|
|1368000000|Iraq|
|1364000000|Iran|
|1352000000|Iceland|
|1380000000|Italy|
|1832000000|Jersey|
|1388000000|Jamaica|
|1400000000|Jordan|
|1392000000|Japan|
|1404000000|Kenya|
|1417000000|Kyrgyzstan|
|1116000000|Cambodia|
|1296000000|Kiribati|
|1174000000|Comoros|
|1659000000|Saint Kitts and Nevis|
|1408000000|North Korea|
|1410000000|The Republic of Korea|
|1414000000|Kuwait|
|1136000000|Cayman Islands|
|1398000000|Kazakhstan|
|1418000000|Laos|
|1422000000|Lebanon|
|1662000000|Saint Lucia|
|1438000000|Liechtenstein|
|1144000000|Sri Lanka|
|1430000000|Liberia|
|1426000000|Lesotho|
|1440000000|Lithuania|
|1442000000|Luxembourg|
|1428000000|Latvia|
|1434000000|Libya|
|1504000000|Morocco|
|1492000000|Monaco|
|1498000000|Moldova|
|1499000000|Montenegro|
|1663000000|saint martin|
|1450000000|Madagascar|
|1584000000|Marshall Islands|
|1807000000|Macedonia|
|1466000000|Mali|
|1104000000|Myanmar|
|1496000000|Mongolia; Mongolia|
|1580000000|Northern Mariana Islands|
|1474000000|Martinique|
|1478000000|Mauritania|
|1500000000|Montserrat|
|1470000000|Malta|
|1480000000|Mauritius|
|1462000000|Maldives|
|1454000000|Malawi|
|1484000000|Mexico|
|1458000000|Malaysia|
|1508000000|Mozambique|
|1516000000|Namibia|
|1540000000|New Caledonia|
|1562000000|Niger|
|1574000000|Norfolk Island|
|1566000000|Nigeria|
|1558000000|Nicaragua|
|1528000000|Netherlands|
|1578000000|Norway|
|1524000000|Nepal|
|1520000000|Nauru|
|1570000000|Niue|
|1554000000|New Zealand|
|1512000000|Oman|
|1591000000|Panama|
|1604000000|Peru|
|1258000000|French Polynesia|
|1598000000|papua new guinea|
|1608000000|The Philippines|
|1586000000|Pakistan|
|1616000000|poland|
|1666000000|Saint Pierre and Miquelon|
|1612000000|Pitt Kaine Islands|
|1630000000|Puerto Rico|
|1275000000|Palestine|
|1620000000|Portugal|
|1585000000|Palau|
|1600000000|Paraguay|
|1634000000|Qatar|
|1638000000|Reunion|
|1642000000|Romania|
|1688000000|Serbia|
|1643000000|Russia|
|1646000000|Rwanda|
|1682000000|Saudi Arabia|
|1090000000|Solomon Islands|
|1690000000|Seychelles|
|1729000000|Sultan|
|1752000000|Sweden|
|1702000000|Singapore|
|1654000000|Helena|
|1705000000|Slovenia|
|1744000000|Svalbard and Jan Mayen|
|1703000000|Slovakia|
|1694000000|sierra leone|
|1674000000|San Marino|
|1686000000|Senegal|
|1706000000|Somalia|
|1740000000|Suriname|
|1728000000|South Sudan|
|1678000000|Sao Tome and Principe|
|1222000000|Salvatore|
|1760000000|Syria|
|1748000000|Swaziland|
|1796000000|Turks and Caicos Islands|
|1148000000|Chad|
|1260000000|French Southern Territories|
|1768000000|Togo|
|1764000000|Thailand|
|1762000000|Tajikistan|
|1772000000|Tokelau|
|1626000000|Timor-Leste|
|1795000000|Turkmenistan|
|1788000000|Tunisia|
|1776000000|Tonga|
|1792000000|Turkey|
|1780000000|Trinidad and Tobago|
|1798000000|Tuvalu|
|1834000000|Tanzania|
|1804000000|Ukraine|
|1800000000|Uganda|
|1581000000|Small islands outside the United States|
|1840000000|U.S.A|
|1858000000|Uruguay|
|1860000000|Uzbekistan|
|1336000000|Vatican|
|1670000000|Saint Vincent and the Grenadines|
|1862000000|Venezuela|
|1092000000|British Virgin Islands|
|1850000000|Virgin Islands|
|1704000000|Vietnam|
|1548000000|Vanuatu|
|1876000000|Wallis and Futuna|
|1882000000|Samoa|
|1887000000|Yemen|
|1175000000|Mayotte|
|1710000000|South Africa|
|1894000000|Zambia|
|1716000000|zimbabwe|
|1000000000|Global|
