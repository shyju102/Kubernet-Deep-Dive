# Kubernetes HostPath creation 

1) Run one web server in your Kubernetes environment image should be httpd
2) create a persistent volume, the name should be a test-PV size  5GB 
3) Claim 1 GB of space and map to the newly created POD under /var/www/html  
4) Host Path location /data 
5) After creating the pod, you need to test the pod, PV, PVC & webpage status


