# Minio Stack
> https://docs.minio.io/docs/deploy-minio-on-docker-swarm

## Create Docker secrets for Minio

```
echo "$(openssl rand -base64 21)" > ACCESS_KEY
echo "$(openssl rand -base64 41)" > SECRET_KEY
cat ACCESS_KEY | docker secret create access_key -
cat SECRET_KEY | docker secret create secret_key -
```

## Labeling Nodes
Minio Stack requires volumes at least 4 and these should be preserved
at restart.
```
docker node update --label-add minio1=true <DOCKER-NODE1>
docker node update --label-add minio2=true <DOCKER-NODE2>
docker node update --label-add minio3=true <DOCKER-NODE3>
docker node update --label-add minio4=true <DOCKER-NODE4>
```

## Deploy
```
docker stack deploy --compose-file=docker-compose-secrets.yaml minio_stack
```

# Minio Client
> https://docs.minio.io/docs/minio-client-complete-guide

## Download client binary
```
wget -O /usr/local/bin/mc https://dl.minio.io/client/mc/release/linux-amd64/mc
chmod +x /usr/local/bin/mc
```

## Add Minio Cloud Storage

```
mc config host add minio <MINIO STACK HOST> <ACCESS_KEY> <SECRET_KEY>
```
* Example:
```
mc config host add minio http://infa-swarm-t1101:10001 $(cat ACCESS_KEY) $(cat SECRET_KEY)
```
