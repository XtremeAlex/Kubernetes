<div style="text-align:center">
<img width="" alt="dashboard" src="_img/dashboard.png">
</div>

# [Kubernetes Dashboard](https://github.com/kubernetes/dashboard)
## E' il client GUI più usato su Kubernetes.

Questa dashboard Web offre una panoramica delle applicazioni in esecuzione sul cluster, nonché permette la creazione o la modifica di singole risorse Kubernetes.

Rispetto ad altri client come Lens e Octant, la sua capacità di filtraggio è limitata.
Non puoi filtrare le risorse per label.

#### La sua configurazione non è così semplice.
Kubernetes Dashboard deve essere installata nel tuo cluster e bisogna gestire alcuni problemi di accesso dell'utente.

La sua configurazione predefinita richiede ogni volta l'accesso tramite il token oppure attraverso il caricamento del file KubeConfig.

Alcuni tutorial suggeriscono di iniettare un proxy di riserva OAuth2 davanti per semplificare il processo di accesso.

Andiamo sul Master e come consigliato dalla pagina github, eliminiamo ogni eventuale precedente versione della dashboard prima di fare il deploy (noi andiamo sempre a utilizzare l’utente kube).

#### Per installarlo eseguire il seguente comando sul master
```
su kube
kubectl delete ns kubernetes-dashboard
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml
kubectl get all –all-namespaces
```

Avremo bisogno di un account amministratore di kubernetes per accedere alla dashboard, in modo tale che abbia i permessi per poter monitorare controllare il cluster, altrimenti potrebbe tornare un errori/warning.

Andiamo quindi a creare lo yaml che ci servirà per la creazione dell’utente, sempre dal master e sempre con l’utente kube (guardare _install_k8s).
```
mkdir -p /kubernates/dashboard
vim /kubernates/dashboard/webkube-dashboard.yml
```

```
apiVersion: v1
kind: ServiceAccount
metadata:ame: web-kube
namespace: kube-system
```

```
kubectl apply -f /kubernates/dashboard/webkube-dashboard.yml
```

Ora dovremo assegnare il ruolo di admin all’utente appena creato per farlo creiamo un nuovo file chiamato ClusterRoleBinding.yml non farà altro che copiare i permessi di un utente che dovrebbe già esistere nel cluster e assegnarli a questo nuovo utente.

```
vim /kubernates/dashboard/ClusterRoleBinding.yml
```

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: web-kube
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
– kind: ServiceAccount
  name: web-kube
  namespace: kube-system
```

```
kubectl apply -f /kubernates/dashboard/ClusterRoleBinding.yml
```

#### Vediamo il token per l’accesso
Prima di provare ad accedere alla dashboard via browser ci serve un ultimo dato, il token segreto per l’accesso con il nuovo utente creato, possiamo risalire al token con questo comando:

```
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep web-kube | awk ‘{print $1}’)
```

Teniamoci da parte questo token.

#### Ora per poter accedere alla dashboard da fuori il cluster dobbiamo creare un tunnelling verso il master, prima però abilitiamo il proxy:

```
kubectl proxy
```

Ora andiamo sul nostro pc e creiamo questo tunnel verso il master, apriamo una shell in locale e usiamo il comando inserendo al posto delle x l'ip del nodo master:

```
ssh -L 8001:127.0.0.1:8001 -N kube@xxx.xxx.xxx.xxx
```

#### Ora possiamo accedere alla dashboard tramite l’indirizzo dal browser
```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

Scegliamo come tipo di accesso “by token” e inseriamo quello dell’utente web-kube trovato e salvato in precedenza.

Fatto l’accesso alla dashboard, possiamo avere il pieno controllo di ogni pod, servizio o deployment.


## Author
`Andrei Alexandru Dabija`

###### Un sincero grazie alla community mi ha permesso di fornirvi questa guida, in caso di ulteriori approfondimenti vi lascio gli url:
- [StackOverflow](https://stackoverflow.com/search?q=kubernates)
- [Techexpert](https://techexpert.tips/it/kubernetes-it/installazione-di-kubernetes-su-ubuntu-linux/)
- [Liquidweb](https://www.liquidweb.com/kb/how-to-install-kubernetes-using-kubeadm-on-ubuntu-18/)
- [Dgline](https://www.dgline.it/digitalroots/guida-per-la-costruzione-di-un-cluster-kubernetes-con-ubuntu/)
