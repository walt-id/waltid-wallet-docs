# Local build

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

Each project can be easily build, using docker:

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

Now launch the containers, using the [docker-compose configuration](../docker-compose.md) described in the previous section, which also sets up the necessary API mappings for the web frontends to find the API endpoints.

### Wallet backend standalone

If you're e.g. a web developer, working on one of the web frontends (**web-wallet**, **issuer-portal**, **verifier-portal**), you may want to run the wallet backend container standalone, to connect your local web frontend build with it.

Edit the configuration files in `waltid-wallet-backend/config`, according to your needs and run the container like so:

```
cd waltid-wallet-backend
docker run -p 8080:8080 -e WALTID_DATA_ROOT=/data -v $PWD:/data waltid/ssikit-wallet-backend
```

_Note:_ Running the web frontend containers standalone, makes little sense, as they depend on an API gateway to connect to the backend APIs. This API gateway is set up by the docker-compose configuration, mentioned above.


## Local build

### wallet-backend

**Requirements**:

* JDK 16+

For importing and building and running the project in your Kotlin/Java IDE, refer to your IDE documentation.

For a plain gradle build, independent of your IDE, simply execute:

```
cd waltid-wallet-backend

# Build:

./gradlew build install

# Now run the backend using the run script:

build/install/waltid-wallet-backend/bin/waltid-wallet-backend
```

### waltid-web-wallet

**Requirements**

* NodeJs v14+
* Yarn 1.22+

**Development build configuration**

In the nuxt configuration in `nuxt.config.js`, you may want to adjust the API proxy mappings, which by default point to either our rolling public deployment or localhost:

Uncomment or adjust to your needs:
```
[...]
proxy: {
   //"/api/": "https://wallet.waltid.org"
   "/api/": "http://localhost:8080"
  },
[...]
```

**Build and run**

Build and run the project using:

```
yarn install
yarn dev
```

The service is started on port **3000** by default.

**Production build**

For production build and other build options, follow the instructions in the `README.md` of the project.

### waltid-issuer-portal

**Requirements**

* NodeJs v14+
* Yarn 1.22+

**Development build configuration**

In the nuxt configuration in `nuxt.config.js`, you may want to adjust the API proxy mappings, which by default point to either our rolling public deployment or localhost:

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

**Build and run**

Build and run the project using:

```
yarn install
yarn dev
```

The service is started on port **8000** by default.

**Production build**

For production build and other build options, follow the instructions in the `README.md` of the project.


### waltid-verifier-portal

**Requirements**

* NodeJs v14+
* Yarn 1.22+

**Development build configuration**

In the nuxt configuration in `nuxt.config.js`, you may want to adjust the API proxy mappings, which by default point to either our rolling public deployment or localhost:

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

**Build and run**

Build and run the project using:

```
yarn install
yarn dev
```

The service is started on port **4000** by default.

**Production build**

For production build and other build options, follow the instructions in the `README.md` of the project.
