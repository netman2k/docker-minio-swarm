# REX-Ray with Minio
* https://rexray.readthedocs.io/en/stable/

## Install REX-Ray binary
```
curl -sSL https://rexray.io/install | sh
```

## Install S3FS FUSE package
```
yum install epel-release -y
yum install s3fs-fuse -y
```

## Generate REX-Ray configuration file
* http://rexrayconfig.cfapps.io/

You need to create a file, `/etc/rexray/config.yml`
and fill contents as below

```
libstorage:
  service: s3fs
s3fs:
  secretKey: <SECRET KEY>
  accessKey: <ACCESS KEY>
  endpoint: http://<MINIO HOST>:<MINIO PORT>
  region: tcc1-dev
  disablePathStyle: false 
  options:
  - url=http://<MINIO HOST>:<PORT>
  - use_path_request_style
  - nonempty
   
```

## Run as a service
```
rexray start
```

## Install Docker plugin
# TODO HERE
```bash
secretkey=$()
docker plugin install rexray/s3fs \
S3FS_OPTIONS="url=<MINIO HOST>:<MINIO PORT>,use_path_request_style,nonempty,allow_other" \
S3FS_ENDPOINT="<MINIO HOST>:<MINIO PORT>" \
S3FS_ACCESSKEY="<ACCESS KEY>" \
S3FS_SECRETKEY="<SECRET KEY>"
```

* Example
```

docker plugin install rexray/s3fs \
S3FS_OPTIONS="url=http://infa-dcoses-t1102:10002,use_path_request_style,nonempty,allow_other" \
S3FS_ENDPOINT="http://infa-dcoses-t1102:10002" \
S3FS_ACCESSKEY="KzefcRicf92pkl+Xc8qJ2lFIqYrG" \
S3FS_SECRETKEY="Lq2BKLUBllo/g7Koyf59GK9HqR3LkL4eql5oIHI+5oCTWO3ZSBXCUv8="
```

## Test
```
docker run -ti --volume-driver=rexray/s3fs -v test:/test busybox
```
