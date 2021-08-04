# infra-aws-nginx-secure

Use the AWS console (web interface) to proceed with 

- create EC2 instance with Public IP on it
- add options in the security group for ssh, ICMP (ping) and http/https

Start the instance and login via command-line (assuming you have the key, etc)

- In your managed hosted zones (Route 53), add a Type A record for a DNS and have that point to your public IP

Note: I had used Amazon Linux EC2 so the steps listed work for that selection.

Install nginx 

```
sudo amazon-linux-extras install -y nginx
sudo systemctl restart nginx
```

Validate that you are able to get data on HTTP.
```
http://<Public-IP>
http://<Route-53-A-Record-name>
```

Install certbot and generate the certificates
```
sudo amazon-linux-extras install -y epel
sudo yum install -y certbot-apache
sudo certbot certonly --standalone --preferred-challenges http -d <Route-53-A-Record-name> --register-unsafely-without-email
```
Reference: https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-centos-7

Update /etc/nginx/nginx.conf
sudo systemctl restart nginx

Test certificate works - on a desktop browser padlock closes
