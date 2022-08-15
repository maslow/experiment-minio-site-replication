


# pre-start

```bash
docker network create laf_shared_network
```

# start services

```bash
  cd ./source && docker-compose down -v && docker-compose up

  # open a new terminal and run
  cd ./target && docker-compose down -v && docker-compose up
```

# enter the source container
```bash
  # open new terminal
  docker exec -it source_s1_1 bash
```

# in container:
```bash
  # !!! replace `arm64` with `amd64`  if you are use x86 cpu 
  curl https://dl.min.io/client/mc/release/linux-arm64/mc \
    --create-dirs \
    -o $HOME/minio-binaries/mc

  chmod +x $HOME/minio-binaries/mc
  export PATH=$PATH:$HOME/minio-binaries/

  mc --version

  # add alias
  mc alias set source http://s1:9000 minio-root-user minio-root-password
  mc alias set target http://t1:9000 minio-root-user minio-root-password

  # enable site replicate
  mc admin replicate add source target

  # tests: create a user in source cluster
  mc admin user add source user1 user-password

  # verify user in target cluster
  mc admin user list target   # should got user1 in list


  # tests: create a bucket in source cluster
  mc mb source/test-bucket

  # verify bucket in target cluster WILL GOT ERROR
  mc ls target/   # should got test-bucket in list (**BUT GOT ERROR HERE**)

```

# clean up

```bash
cd ./source && docker-compose down -v
cd ../target && docker-compose down -v
```