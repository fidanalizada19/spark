# spark
## 1.
##1.
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

## 2.
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm

## 3.

minikube start --cpus='3' --memory='6g' --driver='docker'

## 4.
minikube status

## 5.
sudo rm -rf ~/.minikube/*

## 6 
minikube start

## 7 
 [train@trainvm ~]$ sudo yum -y install java-17-openjdk-devel.x86_64

 ## 8 
  [train@trainvm ~]$ cd /tmp/

## 9
wget https://archive.apache.org/dist/spark/spark-3.5.3/spark-3.5.3-bin-hadoop3.tgz
--2025-02-18 18:05:24--  https://archive.apache.org/dist/spark/spark-3.5.3/spark-3.5.3-bin-hadoop3.tgz

## 10
tar xzf spark-3.5.3-bin-hadoop3.tgz

## 11
export SPARK_HOME=/tmp/spark-3.5.3-bin-hadoop3

## 12
spark-3.5.3-bin-hadoop3/bin/spark-submit --version


## 13 
[train@trainvm dataops]$ mkdir spark_on_minikube

## 14 
cd spark_on_minikube/

## 15 
cat<<EOF | tee spark_on_k8s_app.py
> from pyspark.sql import SparkSession
> import time
>
> spark = SparkSession.builder \
> .appName("Spark on K8s") \
> .getOrCreate()
>
> df = spark.range(1,51)
>
> df.show(50)
>
> time.sleep(30)
>
>
> print("Fidan Alizada")
>
> spark.stop()
> EOF
from pyspark.sql import SparkSession
import time

spark = SparkSession.builder .appName("Spark on K8s") .getOrCreate()

df = spark.range(1,51)

df.show(50)

time.sleep(30)


print("Fidan Alizada")

spark.stop()

## 16 
/tmp/spark-3.5.3-bin-hadoop3/bin/spark-submit \
>     --master k8s://https://192.168.49.2:8443 \
>     --deploy-mode client \
>     --name pyspark-on-k8s \
>     --conf spark.executor.instances=2 \
>     --conf spark.kubernetes.container.image=spark:3.5.3-java17 \
>     local:///home/train/dataops/spark_on_minikube/spark_on_k8s_app.py
