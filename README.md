# Simple Docker Compose for Nginx

This composes an `nginx:alpine` service serving the `levelup` folder and using the local `nginx/nginx.conf`.

Run:

```sh
docker compose up -d
# or 
docker compose -f docker-compose.yml up -d

```

Stop and remove:

```sh
docker compose down
```

Open http://localhost:8080 in your browser.
