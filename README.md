# Maestrano spark-jobserver
Docker image packed with Spark JobServer and MongoDB connector

## Examples
Launch spark jobserver with a list of environment variables
```
docker run --env-file settings/uat.list -p 8090:8090 -d maestrano/spark-jobserver:0.6.2.mesos-0.28.1.spark-1.6.1
```

settings/uat.list file
```
MONGODB_HOST=r0.mongo.uat.maestrano.local
MONGODB_NAME=connec
MONGODB_USER=xxxx
MONGODB_PASSWD=xxxx
```

## shell script that pushes and execute a jar to a docker container running locally
```shell
#!/bin/sh

cd "$(dirname "$0")"

# push SBT build to server
curl --data-binary @../target/scala-2.10/balance-sheet-job_2.10-1.0.jar http://localhost:8090/jars/balance-sheet-job

# Load SQLContext
curl -d "" 'http://localhost:8090/contexts/sql-context?context-factory=spark.jobserver.context.SQLContextFactory'

# Execute jar
curl -d "input.channel_id = org-fbcz" 'http://localhost:8090/jobs?appName=balance-sheet-job&classPath=com.maestrano.impac.spark.jobs.BalanceSheet&context=sql-context&sync=true'

```
