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
6. buat security gruop : nama = sakti-keamanan | ssh = 22 | http = 80 | https = 443 | all-icmp Ipv4 | (all anywhere - IPv4)
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

### edit EC2 nat instance

1. masuk instance ec2
2. code : https://github.com/ibrammanggara/natinstance-ec2-al
-
1. pilih nat instance > action > networking > change source/destination check
2. ceklis stop
3. save

---

## langkah mengkaitkan routable (privat)

### RT-PRIVAT SAKTI
1. #add route
2. destination : 0.0.0.0/0
3. target : nat instance
4. #subnet associations
5. SUBNET PRIVAT 1B
6. SUBNET PRIVAT 1A

---

## testing EC2 private terhubung ke internet melalui nat

pindah ke : https://github.com/ibrammanggara/aws-load-balancer/blob/main/ec2-private.md

---

## langkah membuat launch template (EC2)

1. masukkan nama : ec2-scal
2. ami : amazon linux 2
3. instance type : t2.micro
4. key pair : sakti.pem
5. security grup : sakti-keamanan
6. user data : (code)
7. save

---

## langkah membuat auto scaling (EC2)

1. nama auto scaling : ec2-scaling
2. pilih launch template : ec2-scal
3. next
4. network : vpc=VPC-SAKTI, subnet=(private 1A & privat 1B)
5. next
6. attach to a new load balancer
7. load balancer type : Network Load Balancer
8. nama load balancer = load-balancer-scal
9. scheme : internet-facing
10. load balancer subnet : subnet (publik 1A & publik 1B)
11. port : 80 > create a target group
12. nama target grup : load-balancer-scal
13. Additional health check types : turn on elastic load balancer health checks
14. next
15. Desired capacity : 2
16. Min desired capacity : 1
17. Max desired capacity : 4
18. Target tracking scaling policy : scaling policy name=policy | Metric type=average cpu | target value=30 | Instance warmup=120
19. monitoring (optional)
20. next 3x
21. create auto scaling

---

### testing load balancer ke web browser

















