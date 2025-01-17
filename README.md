# Membuat load balancer dan auto scaling aws

**TAG**: *AWS Load Balancer & Auto Scalling dengan 2 avability zone (n. virginia)*

---

![TPO Gambar](https://raw.githubusercontent.com/ibrammanggara/aws-load-balancer/main/tpo.png)



## Langkah membuat vpc

1. masukan nama vpc : VPC-SAKTI
2. masukan ip cidr IPv4 : 192.168.0.0/16
3. save

---

## Langkah membuat subnet (publik 1-2 & privat 1-2)

1. pilih vpc : VPC-SAKTI
2. masukan nama subnet : PUBLIK 1
3. pilih az : us-east-1a
