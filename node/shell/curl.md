curl基本命令：

```curl
curl \
  -X POST \
  -H "Content-Type: application/json charset=utf-8" \
  "http://localhost:8080/readings/store" \
  -d '{"smartMeterId":"smart-meter-0","electricityReadings":[{"time":1606636800,"reading":0.0503}]}'

```

