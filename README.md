    
    
 # **INITIAL SETUP**
 
 	$ ssh root@{ip-address}
    $ adduser {user}
    $ usermod -aG sudo {user}
    $ cat ~/.ssh/authorized_keys
    $ su - {user}
    $ mkdir ~/.ssh
    $ chmod 700 ~/.ssh
	$ nano ~/.ssh/authorized_keys
	$ chmod 600 ~/.ssh/authorized_keys
    $ exit
    $ nano /etc/ssh/sshd_config
    
      * PasswordAuthentication no
      * PubkeyAuthentication yes
      * ChallengeResponseAuthentication no
      * PermitRootLogin no
    
    $ systemctl reload sshd
    $ ssh {user}@{ip-address}
    $ sudo ufw app list
    $ sudo ufw allow OpenSSH
    $ sudo ufw enable
    $ sudo ufw status
    $ ssh root@{ip-address} => Permission denied (publickey).
    $ ssh {user}@{ip-address} => Successful
    $ sudo nano /etc/sudoers
    
		* {user} ALL=(ALL:ALL) NOPASSWD:ALL
	$ exit
    
# **INSTALL APT PACKAGES**
	
    $ apt-get update
    $ apt-get -y install --fix-missing build-essential
    $ apt-get -y install --fix-missing git
    $ apt-get install --fix-missing htop
    
# **INSTALL NODEJS (NVM)**
	$ curl https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash
    $ source ~/.nvm/nvm.sh
    $ nvm install 6.9.5
    $ ~/.nvm/versions/node/v6.9.5/bin/node -v
    $ sudo ln -s ~/.nvm/versions/node/v6.9.5/bin/node /usr/bin/node
    $ sudo ln -s ~/.nvm/versions/node/v6.9.5/bin/npm /usr/bin/npm
    
# **INSTALL PM2**
	$ npm install pm2 -g --verbose
    $ pm2 startup
    
    * pm2 service actions
  
        $ sudo systemctl enable pm2
        $ sudo systemctl start pm2
        $ sudo systemctl daemon-reload
        $ systemctl status pm2
 # **INSTALL MONGODB**
 	$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    $ echo 'deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse' | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    $ sudo apt-get update
    $ sudo apt-get install -y mongodb-org

    * cli:
        $ sudo service mongod start
        * cli:
        	$ sudo service mongod stop

        * cli:
        	$ sudo service mongod restart
            
    * mongodb settings
    $ mongo
      > use admin; (db seçilebilir)
      > db.system.users.find(); (kullanıcılar listelenebilir)
      * create admin user
        > db.runCommand({createUser:"{Adminuser}", pwd:"{AdminPwd}", roles:[{role:"root", db:"admin"}]});
    $ sudo nano /etc/mongod.conf
  
      security:
        authorization: enabled
    $ sudo service mongod restart
    $ mongo
      > db.system.users.find(); => Error: Unauthorized
      > use admin;
      > db.auth("{Adminuser}", "{AdminPwd}");
      > use {db};  
      > db.runCommand({createUser:"{user}", pwd:"{pwd}", roles:[{role:"readWrite", db:"{db}"}]});
      > use admin;
      > db.system.users.find();
