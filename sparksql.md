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

# SQL Operations

```scala
val sbr_bookings = spark.read.parquet("hdfs://thingy.nce.amadeus.net:8020/anonim_sbr")
sbr_bookings.createOrReplaceTempView("sbr_bookings")

val queryDF = spark.sql("""SELECT creator_office_id,  booking_date, rloc , candidate_tkt_numbers,  tkt_number
FROM sbr_bookings as s
where rloc="12343" 
""")
queryDF.show()
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
