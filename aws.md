#  Deploy the sparta app on AWS


We made an instance, with security groups allowing port 80 and 22 for HTTP and SSH.

We downloaded a pem file &then had to move it to .ssh
``` 
mv ~/Downloads/tech257.pem ~/.ssh/tech257.pem
 ``` 

once there we change the permissions
``` 
chmod 400 tech257.pem
``` 

Then we make a security group


# 1. ssh in
![alt text](<images/Screenshot 2024-03-19 at 12.17.28.png>)

# updates and upgrades

``` 
# Update
sudo apt update -y

#  upgrade for bypassing user input
sudo DEBIAN_FRONTEND=noninteractive apt-get upgrade -yq
``` 

# update config file
sudo vim /etc/needrestart/needrestart.conf
change this:
#$nrconf{restart} = 'i';
to this:
$nrconf{restart} = 'a';

reload config file
sudo sysctl -p

![alt text](<images/Screenshot 2024-03-19 at 12.21.27.png>)


Running rest of commands
``` 
# Install nginx
sudo apt install -y nginx

#reverse proxy 
sudo sed -i "s|try_files .*;|proxy_pass http://127.0.0.1:3000;|g" /etc/nginx/sites-available/default

# Restart nginx
sudo systemctl restart nginx



# Enable nginx
sudo systemctl enable nginx


# Git Clone
git clone https://github.com/Scarlett100/tech257-sparta-app.git

# Install Node.js and npm
curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# CD into app2 folder
cd tech257-sparta-app/app2

# Install npm dependencies
npm install

# Start npm (can also use node.js)
#npm start <-- not needed if doing pm2, because script will never reach pm2 if npm is started.

# Install pm2
sudo npm install pm2@latest -g

#stopPm2 before rerunning.
pm2 kill

# Start app with pm2
pm2 start app.js

``` 

App is working:
![alt text](<images/Screenshot 2024-03-19 at 12.45.27.png>)

app works on desktop
![alt text](<images/Screenshot 2024-03-19 at 12.45.58.png>)

Differences to how I did on azure:

This time I cloned in home directory and did a pm2 kill.











