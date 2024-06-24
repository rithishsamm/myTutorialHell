
# DWINZO MASTER CONTAINER
## OVERVIEW:
Consist of containers, such as:
- UI
- User
- Organization
- Asset Manager
- IoT
- Backend

UI as a master in prod, and services as such User, Organization, Asset Manager, IoT and Backend.

### Use with Docker Development Environments
### React application with a NodeJS backend and a MongoDB database

ERRORS:
Org -> Track ->
> Uncaught runtime errors:
>×
>ERROR
>Cannot read properties of undefined (reading 'machineName')
>TypeError: Cannot read properties of undefined (reading 'machineName')
>   at http://192.168.0.149:3000/static/js/bundle.js:10769:42
> ++
>> more functionalities yet to update
>+++
>> comiple it all to one suite of application 

Project structure:
```
.
├── DwinzoAssetServer
│   ├── Dockerfile
│   ...
├── DwinzoBackendSocket
│   ├── Dockerfile
│   ...
├── DwinzoIoTServer
│   ├── Dockerfile
│   ...
├── DwinzoOrganization
│   ├── Dockerfile
│   ...
├── DwinzoUI
│   ├── Dockerfile
│   ...
├── DwinzoUserServer
│   ├── Dockerfile
│   ...
├── compose.yaml
├── compose-dev.yaml
├── README.Docker.md
└── README.md
```

[_compose.yaml_](compose.yaml)

```
services:
  frontend:
    build:
      context: frontend
      target: development
    ports:
      - 3000:3000
    stdin_open: true
    volumes:
      - ./frontend:/usr/src/app
      - /usr/src/app/node_modules
    restart: always
    networks:
      - react-express
    depends_on:
      - backend

  assetserver:
    restart: always
    build:
      context: assetserver
      target: development
      args:
        NODE_PORT: 8080
    volumes:
      - ./assetserver:/usr/src/app
      - /usr/src/app/node_modules
    depends_on:
      - mongo
    networks:
      - express-mongo
      - react-express
    ports:
      - 8080:8080
    expose: 
      - 8080

  iotserver:
    restart: always
    build:
      context: iotserver
      target: development
      args:
        NODE_PORT: 9000
    volumes:
      - ./iotserver:/usr/src/app
      - /usr/src/app/node_modules
    depends_on:
      - mongo
    networks:
      - express-mongo
      - react-express
    ports:
      - 9000:9000
    expose: 
      - 9000

  organizationserver:
    restart: always
    build:
      context: organizationserver
      target: development
      args:
        NODE_PORT: 5050
    volumes:
      - ./organizationserver:/usr/src/app
      - /usr/src/app/node_modules
    depends_on:
      - mongo
    networks:
      - express-mongo
      - react-express
    ports:
      - 5050:5050
    expose: 
      - 5050
    
  userserver:
    restart: always
    build:
      context: userserver
      target: development
      args:
        NODE_PORT: 3500
    volumes:
      - ./userserver:/usr/src/app
      - /usr/src/app/node_modules
    depends_on:
      - mongo
    networks:
      - express-mongo
      - react-express
    ports:
      - 3500:3500
    expose: 
      - 3500

  backend:
    restart: always
    build:
      context: backend
      target: development
    volumes:
      - ./backend:/usr/src/app
      - /usr/src/app/node_modules
    depends_on:
      - mongo
    networks:
      - express-mongo
      - react-express
    expose: 
      - 3000

  mongo:
    image: mongo
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example

  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8081:8081
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: root
      ME_CONFIG_MONGODB_ADMINPASSWORD: example
      ME_CONFIG_MONGODB_URL: mongodb://root:example@mongo:27017/
      ME_CONFIG_BASICAUTH: false
    volumes:
      - mongo_data:/data/db
    expose:
      - 8081

networks:
  react-express:
    driver: bridge
  express-mongo:
    driver: bridge

volumes:
  mongo_data: 
```