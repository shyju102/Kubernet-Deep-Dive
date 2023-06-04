
https://hub.docker.com/r/minio/minio/

   `velero install   \
   --provider aws    \
   --use-restic   \
   --plugins velero/velero-plugin-for-aws:v1.2.1  \
   --bucket kubedemo    \
   --secret-file minio.credentials    \
   --backup-location-config region=minio,s3ForcePathStyle=true,s3Url=http://10.30.34.51:9000`
   
   `kubectl logs deployment/velero -n velero`
   `velero get backup-location`
  ` velero backup create test-namespacebackup --include-namespaces test --wait`
   `velero backup logs test-namespacebackup`
   `velero get backup-location`
   `velero backup describe test-namespacebackup`
