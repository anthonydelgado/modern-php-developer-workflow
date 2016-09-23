# Modern PHP Development Workflow
Essential Tools For A Modern PHP Development Workflow




# Prerequisites 
These are things you install once per computer (not once per project) and they help you setup and automate your local development enviroment.  
### Install Homebrew (http://brew.sh/)
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
### Install an IDE 
If you can afford it, PHP Storm is a great choice. https://www.jetbrains.com/phpstorm/
Sublime Text is also another great option. I accutally use both! 
```
brew cask install sublime-text
```
### Install Node + NPM 
```
brew install node
```
### WP-CLI (http://wp-cli.org/)
```
brew install homebrew/php/wp-cli
```
### Install Virtual Box & Vagrant
Vagrant uses Virtualbox to manage the virtual dependencies. You can directly download virtualbox and install or use homebrew for it.
```
brew cask install virtualbox && brew cask install vagrant
```
### Install Vagrant-Manager 
Vagrant-Manager helps you manage all your virtual machines in one place directly from the menubar.
```
brew cask install vagrant-manager
```
### Install laravel/homestead vagrant box
Once VirtualBox / VMware and Vagrant have been installed, you should add the laravel/homestead box to your Vagrant installation using the following command in your terminal. It will take a few minutes to download the box, depending on your Internet connection speed:
```
vagrant box add laravel/homestead
```
### Installing Homestead
You may install Homestead by simply cloning the repository. Consider cloning the repository into a Homestead folder within your "home" directory, as the Homestead box will serve as the host to all of your Laravel projects:
```
cd ~

git clone https://github.com/laravel/homestead.git Homestead
```
Once you have cloned the Homestead repository, run the bash init.sh command from the Homestead directory to create the Homestead.yaml configuration file. The Homestead.yaml file will be placed in the ~/.homestead hidden directory:
```
bash init.sh
```
### Editing Homestead.yaml
The folders property of the .Homestead/Homestead.yaml file lists all of the folders you wish to share with your Homestead environment. Below is what your default Homestead.yaml file will look like. 

```
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/Code
      to: /home/vagrant/Code

sites:
    - map: homestead.app
      to: /home/vagrant/Code/Laravel/public

databases:
    - homestead
```

###Mapping Folders
As files within these folders are changed, they will be kept in sync between your local machine and the Homestead environment. You may configure as many shared folders as necessary but I like to use the default ~/Code folder for all my projects. You can leave this as default but it is important to understand what this code is doing, it is mapping your ~/Code folder to the /home/vagrant/Code inside your vitual machine. 
```
folders:
    - map: ~/Code
      to: /home/vagrant/Code
```
Then when cloning repos make sure you are inside your  ~/Code folder so that your folder structure ends up looking something like this:  ~/Code/project-name  ~/Code/another-project-name ect. 

### Adding and Configuring Nginx Sites

Not familiar with Nginx? No problem. The sites property allows you to easily map a "domain" to a folder on your Homestead environment. A sample site configuration is included in the Homestead.yaml file. Again, you may add as many sites to your Homestead environment as necessary. Homestead can serve as a convenient, virtualized environment for every WordPress or Laravel project that you are working on: 
```
sites:
    - map: wordpress-project-name.dev
      to: /home/vagrant/Code/wordpress-project-name
    - map: laravel-project-name.dev
      to: /home/vagrant/Code/laravel-project-name/public
    - map: another-wordpress-project-example.dev
      to: /home/vagrant/Code/another-wordpress-project-example
```
Notice how the Lavel project ends with the folder of /public and the wordpress projects end directly in the root directory of the repo? That is because Laravel, unlike WordPress, keeps most of its logic files outside of the public folder. For most WordPress projects you won't need the /public folder. 
### Editing your Host file

Before your sites new .dev sites will be accessable you will need to ect your HOSTS file. On UNIX based machines this is located at /ect/Hosts. You will have to point each .dev site listed above to your local IP address which should be listed at the top of your .yaml file. 

Open up a terminal and run: 

```
sudo 'nano' /etc/hosts
```

Scroll to the bottom of the file and add one .dev site per line using the following syntax: 

```
192.168.10.10    wordpress.dev
```

When your done Exit the nano editor and save the file by press Control + X to exit: 

```
CTRL + X
```
Then type Y to save and hit enter to over write the file. You will have to do this periotically if you are adding new project dev sites to your machine. 

When you are done your host file should look something like this: 
```
  GNU nano 2.0.6              File: /etc/hosts                                  

##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost

192.168.10.10   homestead.app
192.168.10.10    wordpress.dev
192.168.10.10    anotherawesomeproject.dev
192.168.10.10    websignia.dev 

```


### Adding a Database
At the database section of your Homestead.yaml file, you will find a list of databases. By default, only one called homestead will be listed. You will want to add a database for each project that you work on. Any new line added will enable automatic database creation of the same name.
```
databases:
    - homestead
    - wordpress-project-name
    - laravel-project-name
```    
## Setting up your SSH key

Before you can run vagrant you first have to create an SSH key. To do so run:

```
ssh-keygen -t rsa -C "your@email.com"
```

This will prompt you to enter a paraphrase to creating your ssh key. Make it something you will remember.

then run:

```
pbcopy < ~/.ssh/id_rsa.pub
```

This will copy your ssh key to your clipboard. Next you will go to your git account. Click on the arrow on the top right next to your profile picture and navigate to settings. On the left select SSH AND GPG keys. You will add a new ssh key here by clicking New SSH key and pasting your key into the text box.

You will also need to set up configuration for your git account. To do so run:

```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

## Turning on your Virtual Machine 

Now that we have configured our project in our .yaml file, its time to turn our VM on for the first time. 

To do so, enter into your Homestead directory: 

```
cd ~/Homestead 
```

and run:

```
vagrant up
```

This will read all the setting you set up in your .yaml file and turn on a virtual web server on your local machine. If you add new project sites and/or databases to your .yaml file in the future you will have to restart and provision your VM by running this command inside you Homestead directory: 

```
vagrant reload --provision
```

## Initial Local Project Setup

After you have setup your dev environment and all of your tools it is time to setup you project of the first time. Most modern project won't keep shared libraries inside of thier repos, instead the will be brought together into your porject using a "package manager". One of the most popular package managers is called NPM or Node Package Manager. NPM can help you download and manage all sorts of javascript libraries, everything from client side frameworks such as jQuery and React, to server side frameworks like Express and Socket.io, to local development tools like webpack and gulp.

```
npm install && npm build
```
## Install WordPress from the Command line! 
Using the WP CLI we installed earlier makes setting up a new development enviroment super simple and easy! Make sure you are in the root directory of your project and run: 
```
wp core download
```
This will download all the WordPress core files into your directory. 

To learn more about what WP CLI can do visit: https://wp-cli.org/commands/ 

## Connecting WordPress to your database 



## Bug Fixes 

I had the same issue, my problem was that i run a sudo command that probably affected some permissions, so to fix Homebrew i first run the following command to fix the permissions:
```
sudo chown -R "$USER":admin /usr/local
```
After that I did a cleanup:
```
brew cleanup
```
And I was successfully able to update Homebrew and install packages.
Hope this helps!
