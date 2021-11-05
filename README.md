# Node.js Weight Tracker

![Demo](docs/build-weight-tracker-app-demo.gif)

This sample application demonstrates the following technologies.

* [hapi](https://hapi.dev) - a wonderful Node.js application framework
* [PostgreSQL](https://www.postgresql.org/) - a popular relational database
* [Postgres](https://github.com/porsager/postgres) - a new PostgreSQL client for Node.js
* [Vue.js](https://vuejs.org/) - a popular front-end library
* [Bulma](https://bulma.io/) - a great CSS framework based on Flexbox
* [EJS](https://ejs.co/) - a great template library for server-side HTML templates

**Requirements:**

* [Node.js](https://nodejs.org/) 12.x or higher
* [PostgreSQL](https://www.postgresql.org/) (can be installed locally using Docker)
* [Free Okta developer account](https://developer.okta.com/) for account registration, login

## Install and Configuration

1. Clone or download source files
1. Run `npm install` to install dependencies
1. If you don't already have PostgreSQL, set up using Docker
1. Create a [free Okta developer account](https://developer.okta.com/) and add a web application for this app
1. Copy `.env.sample` to `.env` and change the `OKTA_*` values to your application
1. Initialize the PostgreSQL database by running `npm run initdb`
1. Run `npm run dev` to start Node.js

The associated blog post goes into more detail on how to set up PostgreSQL with Docker and how to configure your Okta account.

# Node.js Weight Tracker 
## How to install on server and use data base server

## 1. Configure the application server

#### git clone  https://github.com/ligum/bootcamp-app  - from the application server, we will call it server 1

#### install on the application server - Node.js LTS (v14)
##### Using Ubuntu server - (we will install version 14, there is an issue with version 16 for this application.)
      curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
      sudo apt-get install -y nodejs

##### we will check the installtion - with node --version or node -v , and also npm
  ![image](https://user-images.githubusercontent.com/41032041/135638342-132e82d9-bfcb-4e58-b982-31a26cdfefcc.png)
![image](https://user-images.githubusercontent.com/41032041/135638581-5594d71f-f026-46c3-9755-bc4851546ac2.png)


#### Next, use npm to initialize the project's package.json file.

    npm init -y

#### Install the project dependencies using the following npm commands.

    npm install
    
    npm install @hapi/hapi@19 @hapi/bell@12 @hapi/boom@9 @hapi/cookie@11 @hapi/inert@6 @hapi/joi@17 @hapi/vision@6 dotenv@8 ejs@3 postgres@1

    npm install --save-dev nodemon@2
    
 
 
### create an .env file
    vim .env
### add your configurtion to the .env file
![ENVfile](https://user-images.githubusercontent.com/41032041/135644267-06eb0624-443a-4d49-ab94-670efd7d23a1.png)

### Now, go to the command line and type the following command to start the development web server.
    npm run dev

### checking that the application is up
![image](https://user-images.githubusercontent.com/41032041/135644889-f240c2e2-b654-4ed4-902d-065ebf3ec1af.png)



### configure that the application will run on boot.
    sudo vim /etc/systemd/system/my-startup.service
   
[Unit]
Description=simple nodejs application server
[Service]
WorkingDirectory=/home/vova/bootcamp-app
ExecStart=/usr/bin/npm run dev
Type=simple
Restart=always
RestartSec=10
[Install]
WantedBy=basic.target
          
![image](https://user-images.githubusercontent.com/41032041/135660824-9366423e-e830-499f-9348-043a6d7c3d28.png)

![the on boot](https://user-images.githubusercontent.com/41032041/135661644-b19d9e6f-d6c9-4feb-8aab-07dd502ab7d5.png)

##### Then we will enable the service
    sudo systemctl enable my-startup.service

##### and we will check the service with - 
    systemctl status my-startup.service
    
sudo systemctl daemon-reload
sudo systemctl enable myapp.service
sudo systemctl start myapp.service
sudo systemctl status myapp.service

![image](https://user-images.githubusercontent.com/41032041/135662310-1cc70bb8-8037-40b5-8151-abaa230e72e6.png)



# 2. Confugure the database server
    sudo apt-get install postgresql postgresql-contrib
    
### the configurtion file is in -
    /etc/postgresql/9.3/main/postgresql.conf

#### checking the service with - 
    systemctl status postgresql
![image](https://user-images.githubusercontent.com/41032041/135656807-89e671cc-8532-4d17-8bda-7eccc571c3fa.png)

##### creating a user in postgresql
    sudo su postgres psql
    ALTER USER postgres WITH PASSWORD 'test1234';
    \q
    psql -U postgres -h localhost
    \q
![image](https://user-images.githubusercontent.com/41032041/135658760-5faceec9-1771-45e0-9184-b61db4002522.png)

###### check that the user was created -
    \du
    npm run initdb

![image](https://user-images.githubusercontent.com/41032041/135658273-c59d226f-5eff-41af-91c2-2257fb66ec67.png)


## Using pgadmin4
#### install pgadmin
    sudo apt install pgadmin4
    sudo /usr/pgadmin4/bin/setup-web.sh
##### and configure user and password there.

#### Access the database with -
    http://your_server_IP/pgadmin4/browser/#
![image](https://user-images.githubusercontent.com/41032041/135660246-9874d9c1-f288-4502-951a-99874e8b7a15.png)

#### Access with the user name and password
![image](https://user-images.githubusercontent.com/41032041/135660441-2e13fbb6-9f0a-41d6-8334-1f05bdfc1afc.png)






# Okta
## We will connect the application with the database in the Okta site.

### create a free developer account - developer.okta.com.
#### Let's create the applicaion

![Okta, create a new application](https://user-images.githubusercontent.com/41032041/135698047-4ba3ccb6-9f41-4d58-8875-55e5adca68fb.png)


![Web application](https://user-images.githubusercontent.com/41032041/135698431-cc4c6c6c-708f-46bc-9107-bcacec94917d.png)

#### choosing the port and application name
![Okta, create a new application2](https://user-images.githubusercontent.com/41032041/135699479-e52e06b5-4b01-40f4-bcee-17a3756a3cac.png)


#### In the .ENV file we created before we enter the Client ID and Client secret
![clientID](https://user-images.githubusercontent.com/41032041/135699020-e30f6b58-7999-46e0-a671-662352180d78.png)

#### The URL for the .ENV file is at the top 
![OKta URL](https://user-images.githubusercontent.com/41032041/135699121-a23ad5a2-59f8-43f9-94b9-a30908ae497b.png)


### Enable Self-Service Registration
#### To allow other people to sign up for an account in your application, you need to enable the self-service registration feature. Click on the Users menu and select Registration.

![Self-Service Registration](https://user-images.githubusercontent.com/41032041/135699393-015d581e-a949-4a16-9eb1-659d60d5e2ed.png)




  
