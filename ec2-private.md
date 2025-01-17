## membuat 1 instance EC2 di privat subnet

1. nama instance : test-internet-privat
2. ami : amazon linux 2
3. instance type : t2.micro
4. key pair : sakti.pem
5. network : vpc=VPC-SAKTI | subnet=SUBNET PRIVAT 1A | security grup=sakti-keamanan
6. storage : 8GB
7. launch instance

---

## upload keypair ke ec2 publik bastion

1. copy code keypair sakti.pem
2. masuk EC2 bastion
3. $nano sakti.pem
4. paste keypair, save
5. $chmod 400 sakti.pem
6. #remote ssh EC2 private dengan ip private
7. $ssh -i "sakti.pem" ec2-user@192.168.20.161
8. coba ping ke google.com
9. $ping google.com

### jika berhasil akan terlihat seperti screen ini

![PING Gambar](https://raw.githubusercontent.com/ibrammanggara/aws-load-balancer/main/ping.png)

### ping berhasil dan jika anda lebih teliti maka akan terlihat saya sedang meremot EC2 private dengan EC2 publik (bastion)
