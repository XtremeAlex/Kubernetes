disponibili<div style="text-align:center">
<img width="720" alt="logo_proxmox" src="_img/logo.png">
</div>


# Costruisci il tuo Cluster Casalingo

## Prerequisiti
- SO Linux / Windows / Mac
- [PROXMOX](https://www.proxmox.com/en/)


#### Installare TRE VM:
-   VM Ubuntu Server (Configurazione minima per garatire un buon funzionamento)

| NOME | CPU | Ram (Mb)|  CPU Conf Consigliata | Ram (Mb) Conf Consigliata | Disco (GB)|
|:--------------|:-------------:|--------------:|--------------:|--------------:|--------------:|
| Kube-Master  | 2 | 4096 | 4 | 8192 | 50 |
| Kube-Slave01 | 1 | 2048 | 2 | 4096 | 50 |
| Kube-Slave02 | 1 | 2048 | 2 | 4096 | 50 |


### Crea una VM
<div>
<img width="" alt="servers" src="_img/screen/1_crea_vm.png">
</div>

### Configurazione di base
<div>
<img width="" alt="servers" src="_img/screen/2_crea_vm.png">
</div>

### Selezioniamo la ISO
<div>
<img width="" alt="servers" src="_img/screen/3_crea_vm.png">
</div>

### Selezioniamo il Disco, preferibilmente SSD
<div>
<img width="" alt="servers" src="_img/screen/4_crea_vm.png">
</div>

### Selezioniamo i core che vogliamo allocare alla vm
<div>
<img width="" alt="servers" src="_img/screen/5_crea_vm.png">
</div>

### Selezioniamo la ram che vogliamo allocare alla vm
<div>
<img width="" alt="servers" src="_img/screen/6_crea_vm.png">
</div>

### Confermare le configurazioni
<div>
<img width="" alt="servers" src="_img/screen/7_crea_vm.png">
</div>

### Clonare il kube-master
 - Rinominiamolo kube-slave01, kube-slave02 ecc...
 - Modificare core e ram ai cloni.

<div>
<img width="" alt="servers" src="_img/screen/8_crea_vm.png">
</div>

#### Visualizzare la guida su `_install_ubuntu/README.md` per la guida su come installare Ubuntu
 - #### [Clicca qui per installare Ubuntu Server](https://github.com/XtremeAlex/Kubernetes/tree/develop/_install_ubuntu)

## Author
`Andrei Alexandru Dabija`
