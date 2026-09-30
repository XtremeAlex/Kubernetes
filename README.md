<div align="center">
<img width="720" alt="Logo Kubernetes" src="_install_k8s/_img/logo.png">
</div>

# Kubernetes

Le guide che avrei voluto trovare quando ho iniziato con Kubernetes e il
DevOps: si parte dalle macchine virtuali, si arriva a un cluster funzionante e
ci si fa girare sopra qualcosa di concreto. Sono pensate per un homelab, ma
quello che si impara vale anche altrove.

## Guide

Nell'ordine in cui conviene seguirle:

1. [Preparare le VM su Proxmox](https://github.com/XtremeAlex/Kubernetes/tree/develop/proxmox): le tre macchine del cluster
2. [Installare Ubuntu Server](https://github.com/XtremeAlex/Linux/tree/main/ubuntu): nel repository Linux
3. [Creare un cluster Kubernetes su Ubuntu](https://github.com/XtremeAlex/Kubernetes/tree/develop/_install_k8s): kubeadm, un master e due worker
4. [Kubernetes Dashboard](https://github.com/XtremeAlex/Kubernetes/tree/develop/dashboard): interfaccia web per il cluster
5. [NGINX su Kubernetes](https://github.com/XtremeAlex/Kubernetes/tree/develop/nginx-k8s): deploy con configurazione personalizzata

In alternativa, per qualcosa di più leggero:

- [K3s su Raspberry Pi](https://github.com/XtremeAlex/Kubernetes/tree/develop/k3s-raspberry): cluster Kubernetes leggero per homelab ed edge (ARM)

> Stato: in parte storico. Le guide 1-5 sono state scritte nel 2021 (Ubuntu 18.04–20.04, kubeadm con
> Docker). I concetti sono validi, ma alcuni comandi sono cambiati: ogni guida
> ha una nota su cosa aggiornare.

## Licenza
Distribuito sotto licenza [Creative Commons Attribution 4.0 (CC BY 4.0)](LICENSE). Puoi condividere e adattare il materiale, anche commercialmente, a condizione di citare l'autore.

## Contatti

Andrei Alexandru Dabija · [LinkedIn](https://www.linkedin.com/in/andrei-alexandru-dabija/) · [github.com/XtremeAlex](https://github.com/XtremeAlex)
