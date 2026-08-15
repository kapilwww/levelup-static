# Docker Compose Setup

This project uses Docker Compose to run two separate Nginx-based services:

- `levelup` on port `9090` -> container port `9000`
- `gym-fitness` on port `8080` -> container port `8000`

Each service uses a custom image and mounts its own default Nginx config from the `nginx-conf` directory.


## Start the stack

```sh
docker compose up -d
```
LEVELUP
To build containers after code changes:


```sh
docker build -t levelup:v0002 -f Dockerfile.levelup .  
```

GYM FITNESS
To build containers after code changes:


```sh
docker build -t gym-fitness:v0001 -f Dockerfile.fitness .  
```

## Stop the stack

```sh
docker compose down
```

## View logs

```sh
docker compose logs -f
```

## Check running containers

```sh
docker ps
```

## Access the apps

- Levelup app: http://localhost:9090
- Gym Fitness app: http://localhost:8080

## Notes

- The config files are mounted read-only into each container.
- If you change the Nginx config files in `nginx-conf`, restart the relevant service:

```sh
docker compose restart levelup
docker compose restart gym-fitness

docker compose down levelup
docker compose down gym-fitness
```
