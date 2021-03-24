<div style="text-align:center">
<img width="720" alt="nginx_logo" src="_img/nginx_logo.png">
</div>

# Installare NGINX su Kubernetes.

Prima di tutto bisogna prendere in considerazione che nginx necessiterà dei alcune configurazioni essenziali e fondamentali per un corretto utilizzo, mi riferisco alle configurazioni `nginx.conf` , ` altra-configurazione.conf `.

### Si può procedere in due modi:

 - #### 1 Creare una ConfigMap Object e montarla

```
mkdir -p /kubernates/nginx-k8s
vim /kubernates/nginx-k8s/deployment.yaml
```

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
data:
  nginx.conf: |
    scrivi
    la
    tua
    configurazione
    principale
    qui
  altra-configurazione.conf: |
    scrivi
    la
    tua
    configurazione
    secondaria
    qui

```

#### Esempio:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
data:
  nginx.conf: |
    user nginx;
    worker_processes  1;
    events {
      worker_connections  10240;
    }
    http {
      server {
          listen       80;
          server_name  localhost;
          location / {
            root   /usr/share/nginx/html; #Change this line
            index  index.html index.htm;
        }
      }
    }

```


- #### 2 Creare un file conf dentro un path `/kubernates/nginx-k8s/nginx.conf` , convertirlo in mappa e poi montarlo:

```
# Configurazione Personalizzata

user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                        '$status $body_bytes_sent "$http_referer" '
                        '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}

```

- #### 2.1 Creare la ConfigMap

```
kubectl create configmap confnginx --from-file=./data/nginx.conf

```


- #### 3 Creare un file YAML con la nuova configurazione del deployment e montare le mappe per i file di configurazione.

```
vim /kubernates/nginx-k8s/deployment.yaml
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
      volumeMounts:
        - name: nginx-config
          mountPath: /etc/nginx/nginx.conf
          subPath: nginx.conf
    volumes:
      - name: nginx-config
        configMap:
          name: confnginx

```

#### Installare il nuovo deployment su Kubernetes.
```
kubectl apply -f /kubernates/nginx-k8s/deployment.yaml
```

#### Verificare i deployment avviati
```
kubectl get deployment
```

#### Creare un file YAML con la nuova configurazione del servizio.
```
vim /kubernates/nginx-k8s/service.yaml
```

```

apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
    app: nginx
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - protocol: TCP
      targetPort: 80
      port: 80
  externalIPs:
    - 192.168.188.120

```

#### Installare il nuovo servizio Kubernetes.

```
kubectl apply -f /kubernates/nginx/service.yaml
```

#### Verificare l'elenco dei servizi Kubernetes.

```
kubectl get services
```

## Author
`Andrei Alexandru Dabija`
