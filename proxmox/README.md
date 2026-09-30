<div align="center">
<img width="720" alt="Logo Proxmox" src="_img/logo.png">
</div>

# Costruisci il tuo cluster casalingo: le VM su Proxmox

Il primo passo per il cluster Kubernetes è avere le macchine. Qui creiamo su
Proxmox le tre VM che faranno da master e da worker: se ne prepara una, poi la
si clona.

## Cosa ti serve

- Un computer con Linux, Windows o macOS da cui gestire Proxmox
- [Proxmox](https://www.proxmox.com/en/) già installato

## Le tre VM

Tutte con Ubuntu Server. La colonna "minimo" è il minimo per far girare le cose
bene; se hai risorse, usa quella consigliata.

| Nome | CPU (minimo) | RAM MB (minimo) | CPU (consigliata) | RAM MB (consigliata) | Disco (GB) |
|:--|:-:|--:|--:|--:|--:|
| Kube-Master  | 2 | 4096 | 4 | 8192 | 50 |
| Kube-Slave01 | 1 | 2048 | 2 | 4096 | 50 |
| Kube-Slave02 | 1 | 2048 | 2 | 4096 | 50 |

## 1. Crea una VM

<img alt="Creazione della VM" src="_img/screen/1_crea_vm.png">

## 2. Configurazione di base

<img alt="Configurazione di base" src="_img/screen/2_crea_vm.png">

## 3. Scegli la ISO

<img alt="Scelta della ISO" src="_img/screen/3_crea_vm.png">

## 4. Scegli il disco, meglio se SSD

<img alt="Scelta del disco" src="_img/screen/4_crea_vm.png">

## 5. Scegli quanti core dare alla VM

<img alt="Core della VM" src="_img/screen/5_crea_vm.png">

## 6. Scegli quanta RAM dare alla VM

<img alt="RAM della VM" src="_img/screen/6_crea_vm.png">

## 7. Conferma la configurazione

<img alt="Riepilogo della configurazione" src="_img/screen/7_crea_vm.png">

## 8. Clona il kube-master

- Chiama i cloni `kube-slave01`, `kube-slave02` e così via.
- Sui cloni abbassa core e RAM secondo la tabella qui sopra.

<img alt="Clonazione della VM" src="_img/screen/8_crea_vm.png">

## Prossimo passo

Installa Ubuntu Server sulle VM seguendo la guida nel repository Linux
(`XtremeAlex/Linux/ubuntu/README.md`):
[installare Ubuntu Server](https://github.com/XtremeAlex/Linux/tree/main/ubuntu).

## Autore

Andrei Alexandru Dabija — [github.com/XtremeAlex](https://github.com/XtremeAlex)
