**Configure pm2 logs with ELK**

**Prerequisite:**

1. We need to Install pm2-elasticsearch-logger to configure pm2 logs with ELK

```bash
   pm2 install pm2-elasticsearch-logger
```

## **Configuration:**

1. Run the following command to configure pm2 logs with ELK

   pm2 set pm2-elasticsearch-logger:(option)(value)

Ex:
```bash
pm2 set pm2-elasticsearch-logger:elasticsearchUri <https://admin:12345@host:9200>
```

2. PM2 will automatically restart the module after changing an option

1. Now run the following command to check the logs
```bash
pm2 logs
```
**Check the pm2 logs in ELK:**

1. After Completion of above steps it will automatically create index pattern in elastic search

![](Aspose.Words.67561974-5a2e-4a1b-8df4-252052c735cd.001.png)

2. Now click on the discover tab on elastic search and select the index pattern to see the logs, then it will show the logs.

![](Aspose.Words.67561974-5a2e-4a1b-8df4-252052c735cd.002.png)
