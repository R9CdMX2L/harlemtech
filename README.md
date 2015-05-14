# Deployment Guide #
This guide is just an overview for deployment of cms and site. Detailed steps are provided on Locomotive CMS website: http://doc.locomotivecms.com/get-started/what-is-locomotivecms

Please note that in provided code snippets, the symbol $ is used for command prompt and it's not necessary to enter this during a console session.


## Install Wagon ##

Extracted from: http://doc.locomotivecms.com/get-started/install-wagon

Wagon is a command line tool for creating LocomotiveCMS sites.

**Step 1: Install ImageMagick**
   $ sudo apt-get install imagemagick

**Step 2: Install Ruby**
I installed Ruby Programming Language using rvm. There are other methods as well and will vary depending on your OS and/or preferences.

    $ curl -L https://get.rvm.io | bash -s stable

This installs ruby devel (e.g. libxml2 required):
Look here for more info: http://stackoverflow.com/questions/6277456/nokogiri-installation-fails-libxml2-is-missing

    $ rvm install ruby-devel-2.0.0

This installs ruby 2.0.0:

    $ rvm install ruby-2.0.0


Exit (end) current terminal session and set the terminal preferences so rvm can be used from gnome terminal. Look here for more details: https://rvm.io/integration/gnome-terminal

Verify that Ruby version was installed.

    $ ruby -v

**Step 3: Install Bundler and Rake **

    $ gem install bundler
    $ gem install rake

**Step 4: Install Wagon**

Install Wagon

    $ gem install locomotivecms_wagon

Verify wagon version.

    $ wagon version


## Install Engine locally ##

**Step 1: Installing mongoDB**

    $ sudo apt-get install mongodb
    $ sudo mkdir /data/db
    $ sudo mongod

This command is useful if shutting down the mongo server:

    $ mongod --shutdown

OR (killing the mongod process)

    $ ps -ef | grep mongod
    $ sudo kill <mongod_process_id>

**Step 2: Installing Ruby on Rails 3**

    $ gem install rails --version=3.2.19

**Step 3: Setting up the Rails app**

    $ rails new acme_cms --skip-active-record --skip-test-unit --skip-javascript --skip-bundle
    $ cd acme_cms

Edit Gemfile and set this contents in:

    source 'https://rubygems.org'
    
    gem 'rails', '3.2.19'
    
    gem 'locomotive_cms', '~> 2.5.6', :require => 'locomotive/engine'
    
    \# Gems used only for assets and not required
    \# in production environments by default.
    group :assets do
      gem 'compass',        '~> 0.12.7'
      gem 'compass-rails',  '~> 2.0.0'
      gem 'sass-rails',     '~> 3.2.6'
      gem 'coffee-rails',   '~> 3.2.2'
      gem 'uglifier',       '~> 2.5.1'
      gem 'therubyracer', :platforms => :ruby
    end
    
    \# Use unicorn as the app server
    gem 'unicorn'

Back in terminal, run:

    $ bundle install

**Step 4: Setup LocomotiveCMS**

    $ bundle exec rails g locomotive:install

Change the name of our mongoid databases in config/mongoid.yml

Edit 'config/initializers/devise.rb' and comment the line that says "config.secret_key =" as there is an error indicating that a method was not found.

Run server

    $ bundle exec unicorn_rails

Open http://localhost:8080 in your web browser

Create account. For example:
Account name: admin
Email: <your email here>
Password admin123
Password confirmation: admin123


##Create your first site##

**Step 1: Generate a site**

    $ mkdir ~/workspace
    $ cd ~/workspace
    $ wagon init my_first_site -t bootstrap3
    $ cd my_first_site
    $ bundle install

**Step 2: Preview site**

    $ bundle exec wagon serve

Open http://localhost:3333/ in your web browser


##Deploy to an engine##

**Pushing the whole site**

    $ bundle exec wagon push hosting

**Push only pages**

    $ bundle exec wagon push hosting --resources=pages

**Push data (force to overwrite existing)**

    $ bundle exec wagon push hosting  --data --force

More info:
http://doc.locomotivecms.com/get-started/deployment


##Troubleshooting##

**libxml2 is missing**
This can be solved by installing lib dependencies and installing build tools:

    $ sudo apt-get install libxslt-dev libxml2-dev
    $ sudo apt-get install build-essential

For more info on this: http://nokogiri.org/tutorials/installing_nokogiri.html


##Credits##
This document was edited using https://stackedit.io/editor

