# Overview

HOWTO for sparksql 2+

# Select Columns

```scala
val res = usersDF.select("name", "favorite_color")
```

# Renaming columns

```scala
val df=parquetFileDF.select($"fuel_surcharge",
                     $"orig_board_point".alias("board_point"),
                     $"orig_off_point".alias("off_point"))
```

# Operations on column

## Concatenations of columns

```scala
df.withColumn("segment",concat($"board_point",$"off_point"))
```

# Save result in csv

```scala
fuels.write.mode("overwrite").format("parquet").save(outputPath)
```
