# if we want to build our own Dockerfile.dev
# !!!! before build, delete installed modules in node_modules directory to prevent copy 2 times
docker build -f Dockerfile.dev -t peterpan01/frontend:latest .

# run container with ports
docker run -p 3000:3000 image_id

# using docker volumes
# pwd: present working directory on my machine (try to write pwd in terminal ==> for windows, let's use POWERSHELL)
# ${PWD}:/app means, map /app folder in running container to PWD in local machine
# -v /app/node_modules : this does not have : -> it means, we don't want to map /app/node_modules of running container to any local machine folder
# WINDOWS: you have to be in POWERSHELL console!!!
# WINDOWS: package.json you have to have script "start": "WATCHPACK_POLLING=true react-scripts start",
# WINDOWS: in POWERSHELL console, cd project_directory
# WINDOWS: check, if you are in project_directory by command: PWD
create image:
docker build -f Dockerfile.dev -t peterpan01/frontend:latest .
docker run -it -p 3000:3000 -v /app/node_modules -v ${PWD}:/app image_id

ponechajme v Dockerfile.dev prikaz na kopirovanie vsetkych files
COPY . .
napriek tomu, ze docker-compose.yml ma tiez nastaveny volume na
    volumes:
      - /app/node_modules
      - .:/app
#pretoze mozno v buducnosti nebudem pouzivat docker-compose, alebo developersky Dockerfile.dev
#budem chciet ako inspiraciu pre produkcny Dockerfile

# Ked chcem na zaklade image spustit ine scripty z package.json ako su defaultne v definovane v image na zaklade Dockerfile.dev CMD ["npm", "run", "start"]
# -it : znamena, ze mozeme po spsteni dalsieho programu v dokri do tohoto programu priamo pisat prikazy
docker run -it image_id npm run test

# Ked chcem spustit script z package.json na zaklade uz beziaceho kontianeru
# skopiruj container_id
docker ps
# spusti script
docker exec -it container_id npm run test

# druha moznost je pridanie novej service do docker-compose.yml
# spusti ju a pre istu prebuidi
docker-compose up --build

# podme sa dostat do beziaceho napr. test containeru
 docker exec -it container_id sh

 # build and run multi-step builds
 docker build -t peterpan01/frontend:latest .
 # nginx has default port 80
 docker run -p 8080:80 [image_id]