# Overview

HOWTO for sparksql 2+

# Select Columns

```scala
val res = usersDF.select("name", "favorite_color")
```

# Save result in csv

```scala
fuels.write.mode("overwrite").format("parquet").save(outputPath)
```
