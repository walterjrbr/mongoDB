# mongoDB
Mostrar a criação de um dataFrame no spark com MongoDB através do pyspark shell


Pré-requistos são as instalações do Spark, MongoDB e Java8

De uma maneira geral iniciar o mongodb na porta padrão 27017 embora pudesse iniciar em outra porta:

# mongod

Executar a chamada no Spark para o conector do MongoDB(conforme documentação no site https://docs.mongodb.com/spark-connector/current/python-api


# $SPARK_HOME/bin/pyspark --packages org.mongodb.spark:mongo-spark-connector_2.11:2.3.1

No prompt do pyspark executar:

>>> from pyspark.sql import SparkSession

my_spark = SparkSession \
    .builder \
    .appName("myApp") \
    .config("spark.mongodb.input.uri", "mongodb://127.0.0.1/test.coll") \
    .config("spark.mongodb.output.uri", "mongodb://127.0.0.1/test.coll") \
    .getOrCreate()


>>> people = spark.createDataFrame([("Bilbo Baggins",  50), ("Gandalf", 1000), ("Thorin", 195), ("Balin", 178), ("Kili", 77),
   ("Dwalin", 169), ("Oin", 167), ("Gloin", 158), ("Fili", 82), ("Bombur", None)], ["name", "age"])
   
   
>>> people.write.format("com.mongodb.spark.sql.DefaultSource").mode("append").save()

Agora você pode acessar o mongoDB

# mongo

Acessar a base onde criou o DataFrame, no caso test
> use test
e acessar a coleção, no caso coll
> db.coll.find()

{ "_id" : ObjectId("5bc0a8444d31c93414961cfd"), "name" : "Bilbo Baggins", "age" : NumberLong(50) }
{ "_id" : ObjectId("5bc0a8444d31c93414961cfe"), "name" : "Gandalf", "age" : NumberLong(1000) }
{ "_id" : ObjectId("5bc0a8434d31c93414961cf7"), "name" : "Oin", "age" : NumberLong(167) }
{ "_id" : ObjectId("5bc0a8434d31c93414961cf8"), "name" : "Gloin", "age" : NumberLong(158) }
{ "_id" : ObjectId("5bc0a8434d31c93414961cf9"), "name" : "Fili", "age" : NumberLong(82) }
{ "_id" : ObjectId("5bc0a8434d31c93414961cfa"), "name" : "Bombur" }
{ "_id" : ObjectId("5bc0a8434d31c93414961cfb"), "name" : "Kili", "age" : NumberLong(77) }
{ "_id" : ObjectId("5bc0a8434d31c93414961cfc"), "name" : "Dwalin", "age" : NumberLong(169) }
{ "_id" : ObjectId("5bc0a8444d31c93414961cff"), "name" : "Thorin", "age" : NumberLong(195) }
{ "_id" : ObjectId("5bc0a8444d31c93414961d00"), "name" : "Balin", "age" : NumberLong(178) }



