# ANOTAÇÕES ESTUDOS KUBERNATES/DOCKER

  
## Comandos docker  

1. Docker build - Criar a imagem
t -> criação de tag, o nome antes da '/' é o repositório no Docker HUB   
f -> apontamento para o arquivo do Dockerfile  
>      docker build -t douglasjava26/curso:v1 -f .\src\Dockerfile .\src\

2. Docker push - Subir imagem para docker hub
"douglasjava26" -> Nome Docker hub
"curso" -> Nome do repositório  
>     docker push douglasjava26/curso:v1

3. Subir imagem Docker
>  docker run -p 8080:8080 douglasjava26/blockchain:v1  

4. Visualizar imagens docker rodando
>  docker container ls

5. Visualizar container que já foram encerrados
>  docker container ls -a

6. Remover containers
>  docker container rm [container id]

7. Iniciar container interativo
>  docker container run -it ubuntu /bin/bash

8. Comando para executar imagem que fica rodando no mesmo cursor
>  docker container run -d nginx

9. Comando para entrar em modo interativo em container já startado
>  docker exec -it [container id] /bin/bash

10. Comando para fazer build da porta interna para uma porta externa
- [-d] -> utilizado para entrar em modulo daemon para não segurar o cursor
- [p] -> Definir uma porta externa primeira porta porta externa usada localmente segunda porta interna no container
>   docker container run -d -p 8080:80 nginx

11. Comando para remover container forçado
>   docker container rm -f 6774019d642f 

12. Comando para parar container 
>   docker container stop 6774019d642f 


apt-get install --yes curl

## Comando kubernates  
  
1. SUBINDO BANCO
f -> apontamento para o arquivo de configuração
>    kubectl apply -f .\k8s\mongodb\service.yaml  
>    kubectl apply -f .\k8s\mongodb\deployment.yaml  
  
2. SUBINDO API
- Fazer deploy dos elementos service/deployment aplicando-os f -> apontamento para o arquivo de configuração
>    kubectl apply -f .\k8s\api\service.yaml  
>    kubectl apply -f .\k8s\api\deployment.yaml  
  
  
3. COMO SEI QUE FOI REALMENTE FEITO O DEPLOY   
>    kubectl get deployments  
  
4. COMO VEJO OS PODS  
>    kubectl get pods  
  
5. COMO RECUPERAR MEU SERVIÇO QUE FOI STARTADO   
>    kubectl get services  
  
6. COMO ESCALAR UM DEPLOYMENT - UM SERVIÇO   
>    kubectl scale deployment api-deployment --replicas=10  
  
7. COMO REMOVER UM POD   
>    kubectl delete pods api-deployment-749d78579f-5f4mj   

8. QUANTOS SERVIDORES ESTÃO SENDO EXECUTADOS  
>    kubectl get nodes   

9. MAIS ESPECIFICO  

>    kubectl get pods -o wide  
  
10. ATUALIZAR IMAGEM DO DEPLOYMENT   
>    kubectl set image deployment api-deployment api=douglasjava26/curso:v2  
  
11. CONFIRMA QUE A IMAGEM FOI ALTERADA  
>    kubectl describe pod   
  
12. VER METRICAS DE CADA POD  
Obs:: é ncessário instalar o metricsservice  --> kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.7/components.yaml  
>    kubectl top pod -->   
  
13. VER QUANTAS VERÕES EXISTEM   
>    kubectl rollout history deployment api-deployment  
  
14. VOLTAR VERSÃO   
>    kubectl rollout undo deployment api-deployment  
  
15. REMOVER SERVICES DO KUBERNATES  
>    kubectl get service -o wide  
>    kubectl delete svc <YourServiceName>  
  
16. VER LOGS  
>    kubectl logs 'nome do pod'  
  
17. Monitoramento  
>    kubectl get hpa  

18. Criar deployment
>    kubectl create deployment nginx --image=nginx

19. Export porta do container
>    kubectl port-forward pod/nginx-f89759699-crpmc 8080:80

20. Detalhes
>    kubectl get all --all-namespaces -o wide

#### FERRAMENTA DE DASHBOARD  
- Octant   

>     Instalação --> choco install octant --confirm  (abrir cmd como ADM)  
  
#### CONFIGURAÇÂO RESOURCES -> deployment  
Configuração realizada para execução do container, requisitar um padrão minimo e um limite para cira outra replica  

    > Requests -> é a configuração minima   
    > Limits -> é a a configuração máxima

> O que acontece quando estoura o container se não tiver configurado.  
> com essa configuração automaticamente ele vai criando mais replicas
> dependendo das requisições realizadas   com a caraga voltando ao
> normal ele mesmo vai removendo as replicas automaticamente, e nesse
> processo   a um aviso para o servidor quando uma replica estiver sendo
> deletado, para ela retirar da fila de loadbalance


## Comandos MULTIPASS

1.  Criação de máquinas virtuais  
n --> especificar o nome da vm  
c quantidade de nucleos  
m quantidade de memória  
d quantidade de disco  

>     multipass launch -n k8s -c 1 -m 2gb -d 20gb  

2.  Acessar internamente a VM  
>     multipass shell k8s  
  
3.  Comando de fora da VM  
>     multipass exec k8s -- comando linux  
  
4.  Montar diretório da máquina física dentro da VM  
>     multipass mount C:Users\Marques k8s:/externo  
  
5.  Remover mapeamento  
>     multipass umount k8s  
  
6.  Recuperar informções sobre as VM  
>     multipass info k8s  
  
7.  Remover pacialmente  
>     multipass delete k8s  
  
8.  Recuperar VM de delete pacialmente  
>     multipass recover k8s  
  
9.  Delete completo  
>     multipass delete k8s  
>     multipass purge  
  
  
## Instalação da VM com microk8s  
>     multipass exec 'name vm criada' -- sudo snap install microk8s --classic --channel=1.18/stable  
>     multipass exec k8s -- sudo usermod -a -G microk8s ubuntu  
>     multipass exec k8s -- sudo chown -f -R ubuntu ~/.kube  
>     multipass restart k8s  

10.  Configurando Kubernate dentro da VM
>     multipass exec k8s -- /snap/bin/microk8s.kubectl create deployment nginx --image=nginx
>     multipass exec k8s -- /snap/bin/microk8s.kubectl get pods
  
11.  Visualizar configurações - Vai imprimir as configuração do kubectl
>     multipass exec k8s -- /snap/bin/microk8s.kubectl config view --raw

12.  Criando mais de um nó com multipass - criar uma nova VM - com o mesmo comando da linha [107] alterando o nome
fazendo toda a configuração aprensetada a partir da linha [135]

13.  Adicionar um nodes - Gerará um comando para criar um join
>     multipass shell 'nome da vm'  
>     microk8s add-node --> Copiar o comando gerado
>     exit
>     multipass shell 'nome da vm'  -> Entra na outra VM
>     colar o comando
>     microk8s remove-node 'nome vm'
>     microk8s leave
>     kubectl get nodes



## Comando kind

1.  Criar cluster com um nó
>     kind create cluster     

2.  Criar cluster nomeado
>     kinf create cluster --name meucluster

3.  Saber quantos cluster tem na máquina
>     kind get clusters

4.  Deletar cluster
>     kind delete cluster --name kind

5.  Criar cluster customizado
É possivel criar arquivos e configuração para criar cluster mais robustos.
> kind create cluster --config .\cluster-one-controll-plane.yaml --name multinodes

>
	kind: Cluster
	apiVersion: kind.x-k8s.io/v1alpha4
	nodes:
	- role: control-plane
	- role: worker
	- role: worker
>	


## Azure CLI AKS
Acessar site abaixo [Versão atual da CLI do Azure]
-- https://docs.microsoft.com/pt-br/cli/azure/install-azure-cli-windows?view=azure-cli-latest&tabs=azure-cli

Teste de instalação 
>  az version

O comando abaixo irá abrir uma página no navegador para a realização do login [Necessário pagar/colocar o cartão] 
>  az login 


1. Criar um cluster kubernatee
- Portal Azure -> https://portal.azure.com
- Menu: All services -> Container -> Kubernates Services -> Adicionar (OBS: Região Brasil constuma-se ser mais caro.)
- Next e configurações (Dúvidas acessar o curso)

2. Acessar cluster criado localmente com [kubectl]
-  kubedev -> nome criado no passo anterior 
-  overwrite-existing -> vai sobreescrever uma possivel configuração antiga localmente
>  az aks get-credentials --resource-group kubedev --name kubedev --overwrite-existing
>  kubectl get nodes


## Scaleway - [Não utilizar em Produção]
1. Criar Cluster
- Portal -> https://www.scaleway.com/en/
- Kubernetes Kapsule 
- Configurações
- Realizar downaload kubeconfig 
- Renomear para config
- Copiar e colar [C:\Users\Marques\.kube] pasta do usuário diretório .kube   
>  kubectl get nodes
>  kubectl create deployment nginx --image=nginx


## Kubeadm
1. Documentação: ​https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/

  

#### POSSÍVEL ERRO NO WINDOWS
  
Falha ao criar VM - timed out   
  
*ERRO*  
-launch failed: The following errors occurred:  
-unassuming-tailorbird: timed out waiting for response  
  
*CORREÇÃO*  
Apagar arquivo: C:\Windows\System32\drivers\etc\hosts.ics  
Desativar Hyper-V   
Reiniciar máquina  
Ativar Hyper-V  
Reiniciar máquina  
Um novo arquivo hosts.ics será criado  
  
Referencias:   
 - https://github.com/canonical/multipass/issues/1512  
 - https://github.com/canonical/multipass/issues/990  
 - https://github.com/canonical/multipass/issues/706