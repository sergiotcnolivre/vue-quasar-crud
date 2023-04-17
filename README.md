# Quasar CRUD Project for Onfly test

## CRUD made with vue3 and quasar framework for Onfly's technical test

### Index page
![index page](https://user-images.githubusercontent.com/51273901/232393334-6f3d17c9-bd6c-43f0-98ca-1dd254e2c60e.png)

### Create user
![create user](https://user-images.githubusercontent.com/51273901/232393529-75184a80-4a87-4762-9a62-775caf4a1edd.png)

### Validation create user
![validation create user](https://user-images.githubusercontent.com/51273901/232393588-a20ef9e3-978f-411b-9f0d-2df63ce3a9ef.png)

### Confirme delete user
![confirm delete](https://user-images.githubusercontent.com/51273901/232393705-aa0df767-e623-4b5a-ae0c-f9b4bc22ba90.png)

### Notify
![user update notfy](https://user-images.githubusercontent.com/51273901/232393734-e57514eb-f980-4692-86f5-493866e07f59.png)

## Set the token

To be able to carry out the requests, go to the quassar.config.js file
search for //env:{}, and replace with:
env:{
TOKEN: 'token value',
},

A Quasar CRUD Project

## Install the dependencies

```bash
yarn
# or
npm install
```

### Start the app in development mode (hot-code reloading, error reporting, etc.)

```bash
quasar dev
```

### Lint the files

```bash
yarn lint
# or
npm run lint
```

### Build the app for production

```bash
quasar build
```

### Customize the configuration

See [Configuring quasar.config.js](https://v2.quasar.dev/quasar-cli-vite/quasar-config-js).
