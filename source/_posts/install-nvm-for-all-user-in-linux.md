---
title: install nvm for all user in linux
date: 2017-11-21 22:16:22
tags: node.js
---

install nvm for all users:

1. Installed nvm in /opt/nvm as root. Seemed like an appropriate location.
  
        # git clone git@github.com:creationix/nvm.git /opt/nvm
  
2. Created the directory /usr/local/nvm. This is where the downloads will go ($NVM_DIR)
  
        # mkdir /usr/local/nvm
  
3. Create the directory /usr/local/node. This is where the NPM global stuff will go:

        # mkdir /usr/local/node
  
4. Created a file called nvm.sh in /etc/profile.d with the following contents:
  
        export NVM_DIR=/usr/local/nvm
        source /opt/nvm/nvm.sh

        export NPM_CONFIG_PREFIX=/usr/local/node
        export PATH="/usr/local/node/bin:$PATH"
  
5. load nvm env
        
        # source /etc/profile.d/nvm.sh
  
6. Re-login to a shell session, then set the default node version.
  
        # nvm ls-remote
        # nvm install 0.10
        # nvm alias default 0.10
