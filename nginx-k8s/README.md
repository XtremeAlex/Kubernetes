<div align="center">
<img width="720" alt="Logo NGINX" src="_img/nginx_logo.png">
</div>

# NGINX su Kubernetes

Far partire NGINX su Kubernetes è facile; il punto è dargli la tua
configurazione (`nginx.conf` ed eventuali file aggiuntivi, qui chiamati
`altra-configurazione.conf`). Il modo giusto è passarla tramite una ConfigMap e
montarla nel container. Si può fare in due modi: scrivere la ConfigMap in YAML,
oppure crearla a partire da un file `.conf`.

## Opzione 1 — ConfigMap scritta in YAML

```
mkdir -p /kubernates/nginx-k8s
vim /kubernates/nginx-k8s/configmap.yaml
```

La struttura è questa: ogni chiave sotto `data` diventa un file.

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

Un esempio concreto:

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

Applicala con `kubectl apply -f /kubernates/nginx-k8s/configmap.yaml`. Se scegli
questa strada, nel deployment più sotto usa `name: nginx-config` al posto di
`confnginx` nella sezione `volumes`.

## Opzione 2 — ConfigMap creata da un file

Scrivi la configurazione in `/kubernates/nginx-k8s/nginx.conf`:

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

e trasformala in una ConfigMap:

```
kubectl create configmap confnginx --from-file=/kubernates/nginx-k8s/nginx.conf
```

## Il deployment

Il deployment monta la ConfigMap al posto di `/etc/nginx/nginx.conf`:

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

Installalo e controlla che sia partito:

```
kubectl apply -f /kubernates/nginx-k8s/deployment.yaml
kubectl get deployment
```

## Il servizio

Per raggiungere NGINX da fuori serve un Service:

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
    - <EXTERNAL_IP>   # sostituisci con l'IP del tuo nodo/cluster
```

Installalo e verifica l'elenco dei servizi:

```
kubectl apply -f /kubernates/nginx-k8s/service.yaml
kubectl get services
```

Nota: `nginx:1.14.2` è la versione degli esempi ufficiali di allora; per un uso
reale scegli un tag aggiornato.

## Autore

Andrei Alexandru Dabija — [github.com/XtremeAlex](https://github.com/XtremeAlex)
