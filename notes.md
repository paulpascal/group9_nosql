```bash
docker exec -it devoir-configsvr1-1 mongosh --port 27201
rs.initiate(
  {
    _id: "confReplSet",
    configsvr: true,
    members: [
      { _id: 0, host: "devoir-configsvr1-1:27201" },
      { _id: 1, host: "devoir-configsvr2-1:27202" },
      { _id: 2, host: "devoir-configsvr3-1:27203" }
    ]
  }
)
```

```bash
docker exec -it devoir-shard1_1-1 mongosh --port 27301
rs.initiate(
  {
    _id: "shard1",
    members: [
      { _id: 0, host: "devoir-shard1_1-1:27301" },
      { _id: 1, host: "devoir-shard1_2-1:27302" },
      { _id: 2, host: "devoir-shard1_3-1:27303" }
    ]
  }
)
```

```bash
docker exec -it devoir-shard2_1-1 mongosh --port 27401
rs.initiate(
  {
    _id: "shard2",
    members: [
      { _id: 0, host: "devoir-shard2_1-1:27401" },
      { _id: 1, host: "devoir-shard2_2-1:27402" },
      { _id: 2, host: "devoir-shard2_3-1:27403" }
    ]
  }
)
```

```bash
docker exec -it devoir-shard3_1-1 mongosh --port 27501
rs.initiate(
  {
    _id: "shard3",
    members: [
      { _id: 0, host: "devoir-shard3_1-1:27501" },
      { _id: 1, host: "devoir-shard3_2-1:27502" },
      { _id: 2, host: "devoir-shard3_3-1:27503" }
    ]
  }
)
```

```bash
docker exec -it devoir-shard4_1-1 mongosh --port 27601
rs.initiate(
  {
    _id: "shard4",
    members: [
      { _id: 0, host: "devoir-shard4_1-1:27601" },
      { _id: 1, host: "devoir-shard4_2-1:27602" },
      { _id: 2, host: "devoir-shard4_3-1:27603" }
    ]
  }
)
rs.reconfig(
  {
    _id: "shard4",
    members: [
      { _id: 0, host: "devoir-shard4_1-1:27601" },
      { _id: 1, host: "devoir-shard4_2-1:27602" },
      { _id: 2, host: "devoir-shard4_3-1:27603" }
    ]
  }
)

rs.stepDown() # election
```

```bash
docker exec -it devoir-mongos-1 mongosh --port 27100
sh.addShard("shard1/devoir-shard1_1-1:27301,devoir-shard1_2-1:27302")
sh.addShard("shard2/devoir-shard2_1-1:27401,devoir-shard2_2-1:27402,devoir-shard2_3-1:27403")
sh.addShard("shard3/devoir-shard3_1-1:27501,devoir-shard3_2-1:27502,devoir-shard3_3-1:27503")
sh.addShard("shard4/devoir-shard4_1-1:27601,devoir-shard4_2-1:27602")
```

# Connect to the MongoDB Cluster

```bash
docker exec -it devoir-mongos-1 mongosh --port 27100
```

# Create a New Database

```bash
show dbs
use dit
sh.enableSharding("dit")
```

# Create a Collection

```bash
db.createCollection("students")
sh.shardCollection("dit.students", { matricule: "hashed" }, false, { "numInitialChunks": 1,
"chunkSize": 10 })

db.students.insert({ name: 'John Doe', matricule: 'AD23'})
show collections

sh.status()
```
