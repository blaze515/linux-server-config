# Udacity Linux Server Config Project

This project involved taking [code developed in a prior project](https://github.com/blaze515/udacity_item_catalog_project/tree/linux-install), putting it on a Linux server, and configuring server security.

### Project Information
| Item | Value |
|---|---|
| IP Address | 18.217.193.115 |
| SSH Port | 2200 |
| App URL | http://www.udacity-project.tk/catalog |

### Summary of Installed Software and Configuration Changes

1) The first step I took for this project was to sign-up for an Amazon Lightsail account to host my linux server and application. I then updated the software packages on the server:
        
        sudo apt-get update
        sudo apt-get upgrade
        
2) After the Ubuntu server was up and running in Lightsail, I made some changes to secure the server and create and configure a grader user.
- Create the grader user:

        sudo adduser grader

- Give the grader user sudo access by adding the following file in the /etc/sudoers.d directory:

        grader ALL=(ALL) NOPASSWD:ALL

- Disabled the standard SSH TCP port and added a custom TCP port (2200) for SSH by adding the following to /etc/ssh/sshd_config:

        Port 2200

- Also configured the sshd_config to use a public CA key:

        TrustedUserCAKeys /home/grader/.ssh/grader_rsa.pub
        
- Setup the UFW firewall:

        sudo ufw default deny incoming
        sudo ufw default allow outgoing
        sudo ufw allow 2200/tcp
        sudo ufw allow www
        sudo ufw allow 123/tcp
        sudo ufw enable

- After making these changes, I was able to login only using SSH. To login as the grader user using the private key, I would use the following command:

        ssh -i ~/.ssh/grader_rsa grader@18.217.193.115 -p 2200

3) After securing the server, I setup my server to work as a web server and configured it to serve my Flask Item Catalog server.

- To get the application server ready, I needed to install several additional Python3 packages--Apache2, WSGI, Flask, PostgresSQL, SQLAlchemy, Git, etc:
        
        // Apache & WSGI Install
        sudo apt-get install apache2
        sudo apt-get install libapache2-mod-wsgi-py3
        
        // PostgresSQL Install
        sudo apt-get install postgresql postgresql-contrib
        
        // Git Install
        sudo apt-get install git-all

        // Pip Install Flask and other supporting sofware
        sudo pip3 install --upgrade pip
        sudo pip3 install sqlalchemy
        sudo pip3 install  flask
        sudo pip3 install httplib2
        sudo pip3 install --upgrade google-cloud
        sudo pip3 install psycopg2

- I also needed to make several configuration changes to my item catalog 
project for it to work properly on the server, such as creating a WSGI conf 
file, updating filepaths, updating the client-secrets.json for Google 
Sign-in, etc. These changes are reflected in the [linux-install branch](https://github.com/blaze515/udacity_item_catalog_project/tree/linux-install) of my item catalog project.
 
 ### Frameworks Used and Other References
 
 - [Website not loading apache conf problems](https://www.digitalocean.com/community/questions/website-not-loading-apache-conf-problems)
 - [How to Setup Apache Hosts on Ubuntu 16.04](https://lowendbox.com/blog/how-to-setup-apache-virtual-hosts-on-ubuntu-16-04/)
 - [Virtual Host Examples](https://httpd.apache.org/docs/current/vhosts/examples.html)
 - [Git Installation](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
 - [Google Credentials](https://cloud.google.com/docs/authentication/)
 - [WSGI and Flask](http://peatiscoding.me/geek-stuff/mod_wsgi-apache-virtualenv/)
 - [Mod WSGI Docs](http://modwsgi.readthedocs.io/en/develop/configuration-directives/WSGIScriptAlias.html)
 - [SQL Alchemy Documentation](http://docs.sqlalchemy.org)
 - [Apache2 Ensite/Dissite](https://manpages.debian.org/stretch/apache2/a2ensite.8.en.html)
 - [Amazon Lighsail](https://lightsail.aws.amazon.com)
 - [TK -- Free Domain](http://www.dot.tk/en/index.html?lang=en)
 - [Iraquitan's Readme -- Referenced when looking into WSGI configuration](https://github.com/iraquitan/udacity-fsnd-p5-linux-server-config)
 - [Multiple Udacity Forum Discussions](https://discussions.udacity.com)
 - [Multiple Stack Overflow Questions](https://stackoverflow.com)



