# infra-aws-nginx-secure

1. Use the AWS console (web interface) to proceed with 
- Creating an EC2 instance (Amazon Linux AMI) with Public IP on it
- Add options in the security group for ssh, ICMP (ping) and http/https

2. Start the instance and login via command-line (assuming you have the key, etc)

3. In your managed hosted zones (Route 53), add a Type A record for a DNS and have that point to your public IP

4. Install nginx 

```
sudo amazon-linux-extras install -y nginx1
sudo systemctl restart nginx
```

5. Validate that you are able to get data on HTTP.
```
http://<Public-IP>
http://<Route-53-A-Record-name>
```

6. Install certbot and generate the certificates
```
sudo amazon-linux-extras install -y epel
sudo yum install -y certbot-apache
sudo systemctl stop nginx
sudo certbot certonly --standalone --preferred-challenges http -d <Route-53-A-Record-name> --register-unsafely-without-email
```
7. Configure the certificates as describe in [Step 3](https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-centos-7). This will lead you to:
- Update /etc/nginx/nginx.conf
- Start back nginx
```
sudo systemctl start nginx
```

8. Test that certificate works! Which means on a desktop browser padlock closes when you access
```
https://<Public-IP>
https://<Route-53-A-Record-name>
```
