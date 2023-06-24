
https://hub.docker.com/r/minio/minio/

# Running minio container
`docker pull minio/minio`

`docker run --name minio -p 9000:9000 -v data:/data minio/minio server /data`

# Offical Documentation Page
https://velero.io/
### Velero installtion 
# Step 1 velero client installation 
Download the velero 
https://github.com/vmware-tanzu/velero/releases/tag/v1.11.0

extract the zip file 

```
scp -r velero /usr/local/bin
```
run #
```
velero version
```



# Step 2
Velero Server installation 

#### Create the secret file, like if you are using any public S3 compatible server, you need to use an access key and a secret key.
If it is MINIO, the use of the login credentials
  ```
[default]
aws_access_key_id=minioadmin
aws_secret_access_key=minioadmin

```
  ```   
velero install --use-node-agent \
    --provider aws \
    --plugins velero/velero-plugin-for-aws:v1.6.1 \
    --bucket kubedemo \
    --secret-file ./minio.credentials \
    --use-volume-snapshots=false \
    --backup-location-config region=minio,s3ForcePathStyle="true",s3Url=http://10.30.34.53:9000
```
 ```
   kubectl logs deployment/velero -n velero
  ```
``` 
   velero get backup-location
   ```
```
   velero backup create test-namespacebackup --include-namespaces test --wait
```
```   
   velero backup logs test-namespacebackup
 ```
```  
   velero get backup-location
 ```
```   
   velero backup describe test-namespacebackup
```
```
velero restore create test --from-backup test-namespacebackup 
```

### Schedule

```
velero schedule create myproject --schedule="* * * * *" --include-namespaces shyju
```
