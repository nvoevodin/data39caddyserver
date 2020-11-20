# Node.js + SSL Deployment with Caddy server

> Steps to deploy a Node.js app to your own (home) server using Caddy as a reverse proxy with automatic SSL certificate 

## 1. Open port 80 on your router
You might need to do this through your internet provider. Optimum has that function on their website in your profile.

## 2. Port Forward port 80 (http) on your router to port 80 on your server IP and same for port 443 (https)
 You can do this by accessing your router settings through a browser.

## 3. Point your godaddy domain to cloudflare

Open cloudflare and add the domain that you bought from godaddy.
In DNS settings create CNAME. Put the name of the prefix that you want. Content should be "yourdomaion.com".
Go to SSL/Tls and set to strict.
Go to overview. Cloudflare should request you to add two dns addresses to godaddy. 
Go to godaddy -> manage dns of your domain -> and switch those to addresses.
Once its done, cloudflare should send you a confirmation email after its active.

## 4. Clone Caddyfile with Docker setup from Github
Clone the caddy and docker compose setup files with:

```
git clone https://github.com/nvoevodin/caddyServer_documentation.git
```
You do not need to touch the docker-compose.yaml
Go to the env -> caddy.env and add your email and your cloudflare global api key
For the api key: Go to profile in your cloudflare account -> api tokens -> and copy your global api key to the caddy.env
Go into the caddyfile and follow the instructions there


## 5. Clone test server from Github
Clone your project. Here is a simple node.js server:
https://github.com/nvoevodin/LetsEncrypt_Documentation.git

```
git clone https://github.com/nvoevodin/LetsEncrypt_Documentation.git #clone

```

It is a hello world server running on port 5000

Once you have docker and docker-compose installed, run:
```
docker-compose up
```

the container will expose a hello world server running on port 5000



### 5. Test if the server app is running

Go to the localhost:5000 in your browser to see 'hello world'



### You should now be able to access your app using your IP and port. Now we want to setup a firewall blocking that port and setup caddy as a reverse proxy so we can access it directly using port 80 (http)

## 6. Setup ufw firewall
On your server computer:

```
sudo ufw enable
sudo ufw status
sudo ufw allow ssh (Port 22)
sudo ufw allow http (Port 80)
sudo ufw allow https (Port 443)
```

## 7. Excecute caddy docker file



### You should now be able to visit your IP with no port (port 80) and see your app. Now let's add a domain

## 9. Add domain in godaddy
Buy a domain

Point the A record to your public IP (just IP address and nothing else after it)

You can find your public ip by typing 'my public ip' in google.

It is different from your usual 192.168.xxx.xx.

It may take a bit to propogate (usually 10-30 mins)

If you were able to see your hello world by visiting your localhost before, you should be able to do the same with your public ip as well. Once godaddy links your domain to that public ip, you should be able to see your hello world by visitin your domain.

10. Add SSL with LetsEncrypt
```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com

# Only valid for 90 days, test the renewal process with
certbot renew --dry-run #using their test server (no renew is really happening)
```

Now visit https://yourdomain.com and you should see your Node app
