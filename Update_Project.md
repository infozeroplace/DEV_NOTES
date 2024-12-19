## Update NodeJS backend

```bash
STEP 1 (LOGIN) : Open any Linux Based Terminal (git bash)
STEP 2 (LOGIN) : ssh root@<ip>
You will be asked for the Password of Server. Enter Password and You will be logged in inside the server.

STEP 3 : For Server Stuff Please Enter in the project folder 
cd /var/www

STEP 4 :  Enter the Project Repo
Example : cd church-logo-server

STEP 5 : Stop the Project running (backend server) using pm2 (package manager)
pm2 status
pm2  stop <id you want to stop>

STEP 6 : take the latest Pull
git  pull origin production

STEP 7 : if any new packages are used in this latest pull use yarn install command again

yarn install
OR
npm install

STEP 8 : Start the server again using the below command

pm2 start “yarn start” –name “<name of project>”

Example.
pm2  start “yarn start”  –name “church-logo-server”


STEP 9 : Check the Backend Server is live again and You are done
```

## Update ReactJS frontend
```bash
STEP 1 (LOGIN) : Open any Linux Based Terminal (git bash)
STEP 2 (LOGIN) : ssh root@<ip>
You will be asked for the Password of Server. Enter Password and You will be logged in inside the server.

STEP 3 : For Server Stuff Please Enter in the project folder 
cd /var/www

STEP 4 :  Enter the Project Repo
Example : cd church-logo-client

STEP 5 : take the latest Pull
git  pull origin production

STEP 6 : if any new packages are used in this latest pull use yarn install command again

yarn/npm install

STEP 7 : Build the project
Example : yarn/npm run build

STEP 7 : Restart nginx
sudo systemctl restart nginx
```

## Update NextJS frontend
```bash
STEP 1 (LOGIN) : Open any Linux Based Terminal (git bash)
STEP 2 (LOGIN) : ssh root@<ip>
You will be asked for the Password of Server. Enter Password and You will be logged in inside the server.

STEP 3 : For Server Stuff Please Enter in the project folder 
cd /var/www

STEP 4 :  Enter the Project Repo
Example : cd church-logo-client

STEP 5 : Stop the Project running (nextjs server) using pm2 (package manager)
pm2 status
pm2  stop <id you want to stop>

STEP 6 : take the latest Pull
git  pull origin production

STEP 7 : if any new packages are used in this latest pull use yarn install command again

yarn/npm install

STEP 8 : Build the project
Example : yarn/npm run build

STEP 9 : start the nextjs project
Example : pm2 start npm --name "well-tea-client" -- start -- -p 3001

STEP 10 : Restart nginx
sudo systemctl restart nginx
```

## Learn Basic Linux Commands
```bash
Learn Basic Linux Commands
exit  : exit from server
ls  : list the files and folders of the current directory
cd .. : change directory to 1 folder back
cd <folder name> : change directory to any directory present in current folder
clear  : clear the terminal
pwd  : present working directory

Pm2 Command
pm2 status

pm2 delete <id1 ,id2, id3>
```