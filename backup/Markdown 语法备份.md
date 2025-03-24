# 一级标题
## 二级标题
### 三级标题
**井号*x x级标题**

`pip install Flask mysql-connector-python`



\``` 代码语言
代码
\```

示例一：Python
```python
#pip install mysql-connector-python

#Provide regional weather in Hong Kong - the latest 10-minute mean wind direction and wind speed and maximum gust
#https://data.gov.hk/en-data/dataset/hk-hko-rss-latest-ten-minute-wind-info
#https://portal.csdi.gov.hk/geoportal/?datasetId=hko_rcd_1634953844424_88011&lang=en

##香港分區天氣 - 最新一分鐘平均氣溫
#"https://data.weather.gov.hk/weatherAPI/hko_data/regional-weather/latest_1min_temperature_uc.csv" 中文
#"https://data.weather.gov.hk/weatherAPI/hko_data/regional-weather/latest_1min_temperature.csv" English

import time
import mysql.connector
import urllib.request
from datetime import datetime


def getWeatherData(url):
    req = urllib.request.Request(url)
    response = urllib.request.urlopen(req).read().decode("utf-8", "ignore").splitlines()
    
    # 讀取 CSV
    weather_data = []
    for line in response[1:]:  # 跳過標題行
        data = line.split(',')
        weather_data.append(data)  # 將每一行數據添加到列表中
    
    return weather_data

# MYSQL connection
serverIP = "127.0.0.1"  # local computer / localhost
dbUser = "root"
dbUserPw = "ICTmysql"

mydb = mysql.connector.connect(host=serverIP, user=dbUser, password=dbUserPw)

# cursor
mycursor = mydb.cursor()
mycursor.execute("USE HKfun;")

# -----------------------------------------

sqlCommand="select date_format(logdate,'%Y-%m-%d %H:%i:%s'), stationnamechi,temperature from hkostation inner join hkotempdata on hkotempdata.stationid = hkostation.stationid order by logid desc limit 39;"
mycursor.execute(sqlCommand)
rows=mycursor.fetchall()
n=len(rows)
print("number of rows returned:",n)
for i in range(n):
    print(str(rows[i][0]),str(rows[i][1]),"Temperature=",str(rows[i][2]),"°C")
    # Remove the call to getWeatherData here
input("End of program")

```

示例二：PHP
```php
<?php
$servername = "127.0.0.1";
$username = "admin";
$password = "abc123";
$dbname = "library2024";

$conn = mysqli_connect($servername,$username,$password,$dbname);
$sql = "SELECT * FROM student";
$result = mysqli_query($conn,$sql);

mysqli_close($conn);
?>

<!DOCTYPE html>

<html>

<body>

<h1>Name list</h1>

<p>Name list of 4A</p>

<table>
  <tr><th>ID</th><th>Name</th><th>Sex</th><th>House</th></tr>

<?php
while ( $row=mysqli_fetch_assoc($result) ) {
  echo "<tr>  <td>".$row["studentid"]."</td>  <td>".$row["studentname"]."</td>  <td>".$row["sex"]."</td>  <td>".$row["house"]."</td>  </tr>";
}
?>

</table>


</body>

</html>


```