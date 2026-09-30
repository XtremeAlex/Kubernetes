<div align="center">
<img width="720" alt="Logo Kubernetes" src="../_install_k8s/_img/logo.png">
</div>

# K3s su Raspberry Pi

Un cluster Kubernetes vero che sta in un cassetto: questa guida installa **K3s**
(la distribuzione Kubernetes leggera di Rancher/SUSE) su Raspberry Pi. È pensata
per homelab ed edge: consuma poco, occupa poco e su ARM gira benissimo.

> Tutti gli indirizzi, gli hostname e i token in questa guida sono **placeholder**
> (`<...>`). Sostituiscili con i tuoi valori e non committare mai dati reali della
> tua rete.

## Perché K3s e non K8s

- Binario unico, memoria ridotta: gira bene su Raspberry Pi 4 (4-8 GB).
- Include già Traefik (ingress), ServiceLB e local-path storage.
- containerd al posto di Docker; SQLite al posto di etcd in single-node.

## Prerequisiti

- Raspberry Pi 4 (o superiore), Raspberry Pi OS Lite 64-bit o Ubuntu Server ARM64
- `cgroup` abilitati: aggiungi a `/boot/cmdline.txt` (su una sola riga):
  ```
  cgroup_memory=1 cgroup_enable=memory
  ```
  poi riavvia.
- IP statico consigliato per il nodo server (assegnalo dal tuo router/DHCP).

## Installazione — nodo server (control-plane)

```bash
curl -sfL https://get.k3s.io | sh -
# stato
sudo systemctl status k3s
sudo kubectl get nodes
```

Recupera il token per aggiungere i worker:

```bash
sudo cat /var/lib/rancher/k3s/server/node-token   # NON committare questo valore
```

## Aggiungere un nodo worker (agent)

Su ogni Raspberry aggiuntivo:

```bash
curl -sfL https://get.k3s.io | \
  K3S_URL=https://<IP_SERVER>:6443 \
  K3S_TOKEN=<NODE_TOKEN> sh -
```

## Accesso da un altro computer

```bash
# sul server
sudo cat /etc/rancher/k3s/k3s.yaml
# copialo sul tuo PC in ~/.kube/config-k3s e sostituisci 127.0.0.1 con <IP_SERVER>
export KUBECONFIG=~/.kube/config-k3s
kubectl get nodes -o wide
```

## Deploy di prova (nginx)

```bash
kubectl create deployment web --image=nginx
kubectl expose deployment web --port=80 --type=LoadBalancer
kubectl get svc web       # ServiceLB assegna un IP dal nodo
```

Con l'ingress Traefik integrato puoi usare un `Ingress` standard senza installare
altro. Per esporre un servizio, definisci un `Ingress` verso il tuo host.

## Storage

La `storageClass` di default è `local-path`: i dati restano sul nodo. Per carichi
stateful su piu nodi valuta uno storage di rete (NFS) invece di local-path.

## Disinstallazione

```bash
# nodo server
/usr/local/bin/k3s-uninstall.sh
# nodo worker
/usr/local/bin/k3s-agent-uninstall.sh
```

## Suggerimenti per Raspberry

- Alimentazione adeguata (5V/3A ufficiale) e SSD USB invece di microSD per durata/prestazioni.
- Fissa la versione di K3s in produzione invece di usare sempre l'ultima.
- Requests/limits sui pod: le risorse su Pi sono scarse.

## Contatti

Andrei Alexandru Dabija (XtremeAlex) · [alexdabi92@gmail.com](mailto:alexdabi92@gmail.com) · [2ad.bubume.it](https://2ad.bubume.it/) · [LinkedIn](https://www.linkedin.com/in/andrei-alexandru-dabija/) · [github.com/XtremeAlex](https://github.com/XtremeAlex)
