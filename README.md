# Membuat load balancer dan auto scaling aws

**TAG**: *AWS Load Balancer & Auto Scalling dengan 2 avability zone (n. virginia)*

---

![TPO Gambar](https://raw.githubusercontent.com/ibrammanggara/aws-load-balancer/main/tpo.png)



## Langkah membuat VPC

1. masukkan nama vpc : VPC-SAKTI
2. masukkan ip cidr IPv4 : 192.168.0.0/16
3. save

---

## Langkah membuat 4 subnet (2 publik subnet & 2 privat subnet)

1. pilih vpc : VPC-SAKTI
2. masukan nama subnet : SUBNET PUBLIK 1A
3. pilih az : us-east-1a
4. masukkan IPv4 subnet CIDR block : 192.168.10.0/24
   ### (+) add new subnet
1. masukan nama subnet : SUBNET PRIVAT 1A
2. pilih az : us-east-1a
3. masukkan IPv4 subnet CIDR block : 192.168.20.0/24
   ### (+) add new subnet
1. masukan nama subnet : SUBNET PUBLIK 1B
2. pilih az : us-east-1b
3. masukkan IPv4 subnet CIDR block : 192.168.30.0/24
   ### (+) add new subnet
1. masukan nama subnet : SUBNET PRIVAT 1B
2. pilih az : us-east-1b
3. masukkan IPv4 subnet CIDR block : 192.168.40.0/24
4. save

   ## edit semua subnet publik
1. Enable auto-assign public IPv4 address
   
---

## membuat internet gateway

1. masukan nama igw : IGW-SAKTI
2. attach ke vpc : VPC-SAKTI

---

## langkah membuat 2 routable (publik dan privat)

 ### publik
1. masukan nama routable : RT-PUBLIK SAKTI
2. pilih vpc : VPC-SAKTI
3. save
 ### privat
1. masukan nama routable : RT-PRIVAT SAKTI
2. pilih vpc : VPC-SAKTI
3. save

---

## langkah mengkaitkan routable (publik)

### RT-PUBLIK SAKTI
1. #add route
2. destination : 0.0.0.0/0
3. target : IGW-SAKTI
4. #subnet associations
5. SUBNET PUBLIK 1B
6. SUBNET PUBLIK 1A

---

## langkah membuat EC2 (publik bastion)

1. masukan nama ec2 : bastion host
2. ami : amazon linux 2
3. instance type : t2.micro
4. key pair : sakti.pem
5. network setting : VPC-sakti, SUBNET PUBLIK 1A, ip publik Enable
6. buat security gruop : nama = sakti-keamanan | ssh = 22 | http = 80 | https = 443 | anywhere - IPv4
7. storage : 8GB
8. save

---

## langkah membuat EC2 (publik nat)

1. masukan nama ec2 : nat instance
2. ami : amazon linux 2
3. instance type : t2.micro
4. key pair : sakti.pem
5. network setting : VPC-sakti, SUBNET PUBLIK 1B, ip publik Enable
6. pilih security gruop : sakti-keamanan
7. storage : 8GB
8. save

---






















