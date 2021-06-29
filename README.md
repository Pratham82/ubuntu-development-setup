# Essential Software Installation

- Get Dotfiles

    ```bash
    git clone https://github.com/Pratham82/dotfiles.git

    # Move all Dotfiles in ~/
    cp .tmux.conf .vimrc .zshrc ~/

    # Copy kitty config
    cp -r dotfiles/.config/kitty/* ~/.config/kitty
    ```

- Shell
    - zsh

        ```bash
        # Install zsh
        sudo apt install zsh

        # change the default shell to zsh
        chsh -s $(which zsh)

        # logout and login again
        ```

    - ohmyzsh

        ```bash
        sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
        ```

    - ohzsh extensions

        ```bash
        # zsh autosuggestions
        git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

        # zsh syntax hughlight
        git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

        # FZF
        git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf

        ~/.fzf/install
        ```

    - spaceship prompt

        ```bash
        # Cloning extensions
        git clone https://github.com/spaceship-prompt/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt" --depth=1

        # Symlink
        ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
        ```

    - tmux, neofetch, htop

        ```bash
        sudo apt install tmux htop neofetch ncdu
        ```

    - lsd

        ```bash
        # Download latest lsd
        https://github.com/Peltoche/lsd/releases

        # Install lsd
        sudo dpkg -i lsd-musl_0.20.1_amd64.deb
        ```

- Terminal
    - Kitty

        ```bash
        sudo apt install kitty
        ```

- Fonts
    - Nerd Fonts

        ```bash
        # Go to NerfFonts folder
        sudo cp -r * /usr/share/fonts
        ```

- Browsers
    - Google chrome

        ```bash
        wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

        sudo dpkg -i google-chrome-stable_current_amd64.deb
        ```

- Programming related
    - Git setup
        - Personal account with SSH keys

            Step 1. Goto .ssh folder and generate ssh keys for all your github accounts

            ```bash
            cd ~/.ssh

            # If no present create one 
            mkdir .ssh
            ```

            Step 2. Make rsa key pair

            ```bash
            # For personal email ID
            ssh-keygen -t rsa -b 4096 -C "mali.prathamesh82@gmail.com"

            # Save as personal ID
            id_rsa_pratham82

            # For work email ID
            ssh-keygen -t rsa -b 4096 -C "prathamesh@edulab.in"

            # Save as work ID
            id_rsa_prathameshedulab

            ```

            Step 3. Copy and paste the RSA key to the respective github account

            ```bash
            cat id_rsa_pratham82.pub
            # Copy the ouput to SSH Keys/ Add new

            cat id_rsa_prathameshedulab.pub
            # Copy the ouput to SSH Keys/ Add new
            ```

            Step 4: Create config file in .ssh folder And add below configs

            ```bash
            vim config

            # Copy paste the following
            # Personal account - default config
            Host github.com-pratham82
               HostName github.com
               User git
               IdentityFile ~/.ssh/id_rsa_pratham82
            # Work account
            Host github.com-prathameshedulab
               HostName github.com
               User git
               IdentityFile ~/.ssh/id_rsa_prathameshedulab
            ```

            Step 5. Make a common .gitconfig

            ```bash
            vim ~/.gitconfig
            # Copy pasete the follwoing in the file
            [user]
                name = pratham82
                email = mali.prathamesh82@gmail.com
            [includeIf "gitdir:~/Work/"]
                path = ~/Work/.gitconfig

            # Making gitcofig for "Work" direcotry
            cd ~/Work
            vim .gitconfig
            [user]
                name = prathameshedulab
                email = prathamesh@edulab.in
            ```

            Step 6. Remove all identities

            ```bash
            # Go to .ssh
            cd ~/.ssh

            # Remove all identites
            ssh-add -D

            # Add the identites
            ssh-add id_rsa_pratham82
            ssh-add id_rsa_prathameshedulab

            # Verify if all identites are added
            ssh-add -l
            ```

            Step 7: Check configuration is right by pinging to github with below commands

            ```bash
            # Check with personl account
            ssh -T github.com-pratham82

            # Check with work account
            ssh -T github.com-prathameshedulab
            ```


    - Languages specific
        - Node (fnm)

            ```bash
            curl -fsSL https://fnm.vercel.app/install | bash -s -- --install-dir "./.fnm" --skip-shell
            ```

        - python3 & pip3

            ```bash
            sudo apt update
            sudo apt install python3-pip
            ```

        - lua

            ```bash
            curl -R -O http://www.lua.org/ftp/lua-5.3.5.tar.gz
            tar -zxf lua-5.3.5.tar.gz
            cd lua-5.3.5/
            make linux test
            sudo make install
            lua

            Download luarocks
            ./configure --with-lua-include=/usr/local/include
            make
            sudo make install
            luarocks install --server=https://luarocks.org/dev luaformatter
            ```


    - DB
        - Mongo

            ```bash
            # Import the public key used by the package management system.¶
            wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -

            sudo apt-get install gnupg

            wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -

            # Create a list file for MongoDB
            echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list

            # Update packages
            sudo apt-get update

            # Install mongo
            sudo apt-get install -y mongodb-org

            # Start mongodb
            sudo systemctl start mongod

            # Verify that MongoDB has started successfully
            sudo systemctl status mongod
            ```

        - MySQL

            ```bash
            https://stackoverflow.com/questions/39281594/error-1698-28000-access-denied-for-user-rootlocalhost
            sudo apt update
            sudo apt install mysql-server

            # Check status
            sudo systemctl status mysql

            # Use secure installation for setting password
            sudo mysql_secure_installation
            ***press N for validating password 

            # Fixing the root login with following commands
            sudo mysql
            use mysql;

            # Check what plugin is used for root auth
            SELECT User, Host, plugin FROM mysql.user;

            # Change root user plugin auth
            ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '9892';
            FLUSH PRIVILEGES;

            ------------------------------------------------------------------
            # Create new user in MySQL
            CREATE USER 'pratham82'@'localhost' IDENTIFIED BY 'rootedpirate';
            # CREATE USER 'pratham82'@'localhost' IDENTIFIED BY '9892';
            GRANT ALL PRIVILEGES ON *.* TO 'pratham82'@'localhost' WITH GRANT OPTION;

            sudo systemctl stop mysql
            sudo apt-get remove --purge mysql-server
            sudo apt-get autoremove
            sudo apt-get autoclean

            # Remove mysql server
            sudo systemctl stop mysql
            sudo apt remove --purge mysql-server
            sudo apt purge mysql-server
            sudo apt autoremove
            sudo apt autoclean
            sudo apt remove dbconfig-mysql
            ```

        - Mongo compass

            ```bash
            https://www.mongodb.com/try/download/compass?tck=docs_compass

            https://downloads.mongodb.com/compass/mongodb-compass_1.26.1_amd64.deb

            sudo dpkg --install mongodb-compass_1.26.1_amd64.deb

            sudo apt --fix-broken install
            ```

        - dbeaver

            ```bash
            https://dbeaver.io/download/

            sudo dpkg -i
            ```

    - Containerization
        - Docker

            ```bash
            # Install docker
            sudo apt-get update
            sudo apt-get install \
                apt-transport-https \
                ca-certificates \
                curl \
                gnupg \
                lsb-release

            # Add Docker’s official GPG key:
            curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

            # Add docker in package list
            echo \
              "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
              $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

            # Add permission to current user
            sudo groupadd docker

            sudo usermod -aG docker ${USER}

            # Install docker-compose
            sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

            sudo chmod +x /usr/local/bin/docker-compose
            ```

    - Other tools
        

        - Remina SSH client

            ```bash
            sudo snap install remmina
            ```

        - Filezilla

            ```bash
            sudo apt update
            sudo apt install filezilla
            ```

    - IDEs & text editors
        - VScode with synced settings

            ```bash
            # Download vscode .db package
            https://code.visualstudio.com/download

            # Install vscode
            sudo dpkg -i code*.deb
            ```

        - Pycharm

            ```bash
            sudo snap install pycharm-community --classic
            ```

    - API related
        - Postman

            ```bash
            sudo snap install postman
            ```

- Multimedia
    - VLC media player

    ```bash
    sudo snap install vlc
    ```
