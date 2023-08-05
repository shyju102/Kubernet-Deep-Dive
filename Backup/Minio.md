https://hub.docker.com/r/minio/minio/

# Running minio container
`docker pull minio/minio`

`docker run --name minio -p 9000:9000 -v data:/data minio/minio server /data`

http://<IPADDRESS>:9001/

Default username : minioadmin
password : minioadmin
