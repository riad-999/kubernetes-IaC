# Multi-tier web app deployment automated with Yaml files and Bash script


In this project the deployment consist of : 

1.  React (frontend)
2.  Nodejs (Backend)
3.  Redis (Cache + Pub/Sub) caching capabilities and real-time communication through Pub/Sub.
4.  Mysql Database
5.  metrics exporters: node exporter, redis exporter
6.  prometheus & grafana for monitoring the metrics

### **Backend Part**
- the file `api-server.yaml` contains the deployment details and the service and HPA for the backend pods.
- the file `worker-server.yaml` contains the deployment details and the service for the backend workers (redis subscribers) pods.
- `app-config.yaml` is a config-map the contains env varialbes used by the backend services.
- `approle.yaml` contains the role and role binding to allow the backend service accound to access the secrets (Mysql and redis passwords).

### **Redis**
- `redis-primary.yaml` is the deployment of the primary write instance with its service.
- `redis-replica.yaml` is th deployment of the read replica instance with its service.

### **MYSQL Database**
- `mysql-statefullset.yaml` is a statefull deployment for mysql.
- `local-sc.yaml` is the storage class that mysql statefullset uses to create the persistent volume.
- `mysql-service.yaml` is a headless service to expose a fixed mysql entrypoint to the backend services.
- `init.sql` is the sql file the `deplpy.sh` uses to create a config map with will be used to create once the pod is launched the database structure. 

### **Frontend Part**
- the file `client.yaml` contains the deployment details and service for the frontend part.

### **Metrics Exporters**
- `node-exporter`is the node exporter deamonset that monitors the metrics of each kubernets node.
- `redis-exporter.yaml` are the exporters of redis, one for the primary instance and the other for the read replcia.
- there is also an exporter built in in node js that exports metric by default on the /metrics endpoints of the server.

### **prometheus & Grafana**
- `prometheus.yaml` is the the deployment of prometheus along with its config-map and service that gets the metrics from all the exporters
- `grafana.yaml` is the deployment of grafana with its service
