# Demonstration to accompany ".net core on linux" talk.
#
# assumptions: 
# * you've already signed up to digital ocean
# * you've already created a public/private key pair (for example by using PuTTYgen or ssh-keygen)
# * you've given digitalocean the public key to this pair.

Get a droplet

# https://cloud.digitalocean.com/droplets/new?size=2gb&region=nyc3
# Take note of the version of Ubuntu, e.g. 16.04
# Pick the smallest size. Make sure 64 bit.
# (Why? because you guys are worth it.)
# "Add your SSH Keys" pick one you've added already
# Give the droplet a name: ddd-test-1. Hit create.
# When it is finished creating after a minute... copy the ipaddress, e.g.: 138.197.25.125

Configure droplet...

# much of this is based on: https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04
# can now connect to your digital ocean droplet
# in 4 different ways:
# * using the browser (https://cloud.digitalocean.com/droplets -> more -> Access console)
# * using ssh with putty
# * using ssh with 'the linux subsystem for windows' (bash.exe)
# * using ssh in powershell
# for the demo i use bash.exe.

http://138.197.25.125

# visit it in the browser... we see nothing.
# now ssh onto it

ssh root@138.197.25.125

# are you sure... enter "yes"
# enter passphrase (if you have one on your keypair. i do)

# create new user who is not root

sudo adduser leon

# enter password (twice)
# press enter on everything else

# give new user sudo privileges

sudo usermod -aG sudo leon

# to remove user:  userdel leon
# copy your public key from local machine to that new user...

ssh-copy-id leon@138.197.25.125

# reconnect via ssh as our low privilege user

ssh leon@138.197.25.125

## disable password authentication on your server

sudo nano /etc/ssh/sshd_config

# prevent ssh connections from root

PermitRootLogin no

# prevent ssh connections via passwords
# uncomment the line PasswordAuthentication  -- and change "yes" to "no"

PasswordAuthentication no

# save with Ctrl O, exit with Ctrl X

see also: ufw and fail2ban

# further reading to harden your servers:
# [An Introduction to Securing your Linux VPS (Digital Ocean)](https://www.digitalocean.com/community/tutorials/an-introduction-to-securing-your-linux-vps)
# [7 Security Measures to Protect Your Servers (Digital Ocean)](https://www.digitalocean.com/community/tutorials/7-security-measures-to-protect-your-servers)

# reload the ssh daemon

sudo systemctl reload sshd

# Password authentication is now disabled. Your server is now only accessible with SSH key authentication


# Part 2... installing .net
# Based mostly on https://docs.microsoft.com/en-us/aspnet/core/publishing/linuxproduction


install .net core

#

sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'

# enter password

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893

# the above steps are just to update linux's knowledge of where to look for packages.

sudo apt-get update

# now to install .net core itself...

sudo apt-get install dotnet-dev-1.0.0-preview2.1-003177

#

"hello world"

# cd
# mkdir hello
# cd hello

dotnet new

# first time this is run it can take up to a minute!

dotnet restore
dotnet run

# cd

create sample web app

# mkdir webapp
# cd webapp

dotnet new -t web

# install `tree` so we can look at the structure of this app...

sudo apt-get install tree

# these are the folders...

tree -d

# run the app -- TODO: count the packages

dotnet restore
dotnet run


# from another window...

curl http://localhost:5000

# Try visiting it in the browser..  no success.

http://138.197.25.125


#install nginx

sudo apt-get install nginx

#

sudo service nginx start

#Now if you visit your ip address in any browser, you'll see it!
# e.g. 

http://138.197.25.125/

# but it won't be the site you created, just nginx's default page

# look at the nginx config:

cd /etc/nginx/sites-available
cat default

# make a backup of default 
#sudo cp default default.backup

sudo rm default
sudo touch default
sudo nano default

# delete it and re-create an empty file
# then edit it... 


# to contain the following...

server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

# check your edits are ok
sudo nginx -t

# reload the nginx service...
sudo nginx -s reload



# 
http://138.197.25.125

# now you can visit the site from anywhere and hopefully see our website...
# but if we close that terminal, the website will terminate.
# need to use something else to manage the process.
# analogous to the way IIS has "application pools" that are kept running.
# we can use supervisor or systemd

#
# TODO: configure systemd to keep our app running
# ~/webapp/bin/Debug/netcoreapp1.1/webapp.dll  (note -- should locate the site in /var/aspnetcore/appname}

sudo nano /etc/systemd/system/kestrel-webapp.service

# note, can instead go --     ExecStart=/usr/bin/dotnet /home/leon/webapp/bin/Debug/netcoreapp1.1/webapp.dll

[Unit]
    Description=example website on ubuntu

    [Service]
    ExecStart=/usr/bin/dotnet run
    WorkingDirectory=/home/leon/webapp 
    Restart=always
    RestartSec=10
    SyslogIdentifier=dotnet-webapp
    User=leon
    Environment=ASPNETCORE_ENVIRONMENT=Production 

    [Install]
    WantedBy=multi-user.target

#

systemctl enable kestrel-webapp.service
# enter password twice... once to manage-unit-files, once to reload-daemon

systemctl start kestrel-webapp.service

# has to enter password again

# systemctl stop kestrel-webapp.service

# any problems you may need to reload the daemon...
# systemctl daemon-reload
# or look at the log here:
# sudo journalctl -fu kestrel-webapp.service


systemctl status kestrel-webapp.service

# look for green text: "active (running)" -- 
# Can now visit the site http://138.197.25.125
# And see the website running

# T: 21
    
# TODO: back in visual studio ----
# create an app 
# publish it
# copy it to the device
# run it
