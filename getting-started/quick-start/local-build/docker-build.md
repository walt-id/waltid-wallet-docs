# Docker Build

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

Now, launch the containers using the [docker-compose configuration](../docker-compose.md) described in the previous section, which also sets up the necessary API mappings for the web frontends to find the API endpoints.
