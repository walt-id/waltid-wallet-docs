# Local Build

## Wallet Backend (wallet-backend)

For building the project **JDK 16+** is required.

For importing, building and running the project in your Kotlin/Java IDE, refer to your IDE documentation.

For a plain Gradle build, independent of your IDE, simply execute:

```
cd waltid-walletkit

# Build:

./gradlew build install

# Now run the backend using the run script:

build/install/waltid-walletkit/bin/waltid-walletkit run
```

## Web Wallet (waltid-web-wallet)

For building the project **NodeJs v14+** and **Yarn 1.22+** is required.

In the **nuxt configuration** in `nuxt.config.js`, you may want to adjust the API proxy mappings, which by default point to either our rolling public deployment or localhost:

Uncomment or adjust to your needs:

```
[...]
proxy: {
   //"/api/": "https://wallet.walt-test.cloud"
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

## Issuer Portal (waltid-issuer-portal)

For building the project **NodeJs v14+** and **Yarn 1.22+** is required.

In the **nuxt configuration** in `nuxt.config.js`, you may want to adjust the API proxy mappings, which by default point to either our rolling public deployment or localhost:

Uncomment or adjust to your needs:

```
[...]
proxy: {
    //'/issuer-api/': 'https://wallet.walt-test.cloud',
    //'/onboarding-api/': 'https://wallet.walt-test.cloud',
    //'/api/': 'https://wallet.walt-test.cloud'
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

## Verifier Portal (waltid-verifier-portal)

For building the project **NodeJs v14+** and **Yarn 1.22+** is required.

In the **nuxt configuration** in `nuxt.config.js`, you may want to adjust the API proxy mappings, which by default point to either our rolling public deployment or localhost:

Uncomment or adjust to your needs:

```
[...]
proxy: {
    // '/verifier-api/': 'https://wallet.walt-test.cloud',
    // '/api/': 'https://wallet.walt-test.cloud'
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
