# Udacity Linux Server Config Project

This project involved taking (code developed in a prior project)[https://github.com/blaze515/udacity_item_catalog_project/tree/linux-install], putting it on a Linux server, and configuring server security.

### Project Information
| Item | Value |
|---|---|
| IP Address | 18.217.193.115 |
| SSH Port | 2200 |
| App URL | http://www.udacity-project.tk/catalog |

### Summary of Installed Software and Configuration Changes

1) The first step I took for this project was to sign-up for an Amazon Lightsail account to host my linux server and application. I then updated the software packages on the server:
'''
sudo apt-get update
sudo apt-get upgrade
'''

2) After the Ubuntu server was up and running in Lightsail, I made some changes to secure the server and create and configure a grader user.
- Create the grader user:
'''
sudo adduser grader
'''

- Give the grader user sudo access by adding the following file in the /etc/sudoers.d directory:
'''
grader ALL=(ALL) NOPASSWD:ALL
'''

- Disabled the standard SSH TCP port and added a custom TCP port (2200) for SSH by adding the following to /etc/ssh/sshd_config:
'''
Port 2200
'''

- Also configured the sshd_config to use a public CA key:
'''
TrustedUserCAKeys /home/grader/.ssh/grader_rsa.pub
'''

- After making these changes, I was able to login only using SSH. To login as the grader user using the private key, I would use the following command:
'''
ssh -i ~/.ssh/grader_rsa grader@18.217.193.115 -p 2200
'''

3) After securing the server, I setup my server to work as a web server and configured it to serve my Flask Item Catalog server.

- To get the application server ready, I needed to install several additional Python3 packages--Apache2, WSGI, Flask, PostgresSQL, SQLAlchemy, Git, etc:

'''
sudo apt-get install apache2
sudo apt-get install libapache2-mod-wsgi-py3
sudo apt-get install postgresql postgresql-contrib
sudo apt-get install git-all

sudo pip3 install --upgrade pip
sudo pip3 install sqlalchemy
sudo pip3 install  flask
sudo pip3 install httplib2
sudo pip3 install --upgrade google-cloud
sudo pip3 install psycopg2
'''

-



