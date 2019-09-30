It's only for dev purpose not for production.

HOW to use it:
1) create folders on the same level as fullstack_docker catalog:
- backend
- frontend

1a) inside of frontend create folders:
angular-app, react-app
because if you don't do it, you will not have writing permission in it

2) build:
docker-compose build
docker-compose up -d

3) for entering symfony server type:
docker exec -it -u user php bash

4) for entering node server type:
docker exec -it node bash
4a) for running angular app go to the angular-app folder and app type:
ng serve --host 0.0.0.0
4b) for running react app go to the react-app folder and type:
npm start

5) In browser type localhost:8080
