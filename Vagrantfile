# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # speed up apt-get
  # config.cache.auto_detect = true

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/xenial64"

  #Fix
  config.ssh.shell = "bash -c 'BASH=ENV=/etc/profile exec bash'"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.

  # config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.network "forwarded_port", guest: 8000, host: 8000
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.network "forwarded_port", guest: 5432, host: 5432
  # Debugging Port
  config.vm.network "forwarded_port", guest: 5678, host: 5678

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
        ##############################
        #     Apt-get Keys           #
        ##############################
        wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
        sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
        curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
        sudo echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
        # updates the list of available packages and their versions, but it does not install or upgrade any packages.
        sudo apt-get -qq update
        # actually installs newer versions of the packages you have. After updating the lists, the package manager knows about available updates for the software you have installed.
        sudo apt-get -qq upgrade
        # Install all dependencies for build-essential (contains numerous packages for building packages)
        sudo apt-get -qq build-dep build-essential
        # Install git
        sudo apt-get -qq build-dep git
        ##############################
        # Yarn and Node Installation #
        ##############################
        sudo apt-get install yarn
        curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
        sudo apt-get install -qq nodejs
        # Install Source Code Pro font
        sudo apt-get -qq install fontconfig
        [ -d /usr/share/fonts/opentype ] || sudo mkdir /usr/share/fonts/opentype
        sudo git clone https://github.com/adobe-fonts/source-code-pro.git /usr/share/fonts/opentype/scp
        sudo fc-cache -f -v
        ##############################
        #     ZSH Installation       #
        ##############################
        # Added zsh shell.
        sudo apt-get -qq install zsh
        wget --no-check-certificate https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
        sudo chsh -s /bin/zsh vagrant
        zsh
        # Change the oh my zsh default theme.
        sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="ys"/g' ~/.zshrc
        ##############################
        #     Ultimate .vimrc        #
        ##############################
        # Adding ultimate .vimrc configs
        sudo git clone git://github.com/amix/vimrc.git ~/.vim_runtime
        sudo sh ~/.vim_runtime/install_awesome_vimrc.sh
        ##############################
        #   PostgreSQL Install       #
        ##############################
        sudo apt-get -qq build-dep postgresql
        ##############################
        #   Jenkins Install          #
        ##############################
        sudo apt-get -qq install jenkins
        ##############################
        #   Anaconda                 #
        ##############################
        sudo apt-get install python3-pip
        sudo wget https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh
        sudo bash Anaconda-2.3.0-Linux-x86_64.sh

        vagrantTip="The shared directory is located at /vagrant\nTo access your shared files: cd /vagrant"
        echo -e $vagrantTip > /etc/motd
   SHELL
  # Change the vagrant user's shell to zsh
  config.vm.provision :shell, inline: "chsh -s /bin/zsh vagrant"

  #config.vm.provision :shell, :path => "postgresql_provisions.sh"
  #config.vm.provision :shell, :path => "python_provisions.sh"

  # For shared folders:
  # config.vm.synced_folder "www", "/var/www"
  
end
