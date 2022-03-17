# Local Build

## Clone source

In order to build the wallet or one of the affiliated projects, you have to clone the project source on your local machine, e.g.:

```
mkdir waltid
cd waltid
git clone https://github.com/walt-id/waltid-wallet-backend.git
git clone https://github.com/walt-id/waltid-web-wallet.git
git clone https://github.com/walt-id/waltid-issuer-portal.git
git clone https://github.com/walt-id/waltid-verifier-portal.git
```

## Docker build

Each project can be easily build using docker:

```
cd waltid-wallet-backend
docker build --rm -t waltid/ssikit-wallet-backend .

cd ../waltid-web-wallet
docker build --rm -t waltid/ssikit-web-wallet .

cd ../waltid-issuer-portal
docker build --rm -t waltid/ssikit-issuer-portal .

cd ../waltid-verifier-portal
docker build --rm -t waltid/ssikit-verifier-portal .
```

Now, launch the containers using the [docker-compose configuration](docker-compose.md) described in the previous section, which also sets up the necessary API mappings for the web frontends to find the API endpoints.

## Wallet backend standalone

If you are working on one of the web frontends (e.g. web-wallet, issuer-portal, verifier-portal), you may want to run the wallet backend container standalone, to connect your local web frontend build with it.

Edit the configuration files in `waltid-wallet-backend/config`, according to your needs and run the container like so:

```
cd waltid-wallet-backend
docker run -p 8080:8080 -e WALTID_DATA_ROOT=/data -v $PWD:/data waltid/ssikit-wallet-backend run
```

Note: Running the web frontend containers standalone, makes little sense, as they depend on an API gateway to connect to the backend APIs. This API gateway is set up by the docker-compose configuration, mentioned above.

## Local build

### Wallet Backend (wallet-backend)

For building the project **JDK 16+** is required.

For importing, building and running the project in your Kotlin/Java IDE, refer to your IDE documentation.

For a plain Gradle build, independent of your IDE, simply execute:

```
cd waltid-wallet-backend

# Build:

./gradlew build install

# Now run the backend using the run script:

build/install/waltid-wallet-backend/bin/waltid-wallet-backend run
```

### Web Wallet (waltid-web-wallet)

For building the project **NodeJs v14+** and **Yarn 1.22+** is required.

In the **nuxt configuration** in `nuxt.config.js`, you may want to adjust the API proxy mappings, which by default point to either our rolling public deployment or localhost:

Uncomment or adjust to your needs:

```
[...]
proxy: {
   //"/api/": "https://wallet.waltid.org"
   "/api/": "http://localhost:8080"
  },
[...]
```

**Build and run** the project using:

```
yarn install
yarn dev
```

The service is started on port **3000** by default.

For **production build** and other build options, follow the instructions in the `README.md` of the project.

### Issuer Portal (waltid-issuer-portal)

For building the project **NodeJs v14+** and **Yarn 1.22+** is required.

In the **nuxt configuration** in `nuxt.config.js`, you may want to adjust the API proxy mappings, which by default point to either our rolling public deployment or localhost:

Uncomment or adjust to your needs:

```
[...]
proxy: {
    //'/issuer-api/': 'https://wallet.waltid.org',
    //'/onboarding-api/': 'https://wallet.waltid.org',
    //'/api/': 'https://wallet.waltid.org'
    '/issuer-api/': 'http://localhost:8080/',
    '/onboarding-api/': 'http://localhost:8080/',
    '/api/': 'http://localhost:8080/'
  },
[...]
```

**Build and run** the project using:

```
yarn install
yarn dev
```

The service is started on port **8000** by default.

For **production build** and other build options, follow the instructions in the `README.md` of the project.

### Verifier Portal (waltid-verifier-portal)

For building the project **NodeJs v14+** and **Yarn 1.22+** is required.

In the **nuxt configuration** in `nuxt.config.js`, you may want to adjust the API proxy mappings, which by default point to either our rolling public deployment or localhost:

Uncomment or adjust to your needs:

```
[...]
proxy: {
    // '/verifier-api/': 'https://wallet.waltid.org',
    // '/api/': 'https://wallet.waltid.org'
     '/verifier-api/': 'http://localhost:8080/',
     '/api/': 'http://localhost:8080/'
  },
[...]
```

**Build and run** the project using:

```
yarn install
yarn dev
```

The service is started on port **4000** by default.

For **production build** and other build options, follow the instructions in the `README.md` of the project.
