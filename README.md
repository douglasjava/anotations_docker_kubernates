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

13. Comando para baixar uma imagem, só download não executa
>   docker pull ubuntu

14. Comando para remover imagem, para isso o container tem que ser parado, não pode está rondando
>   docker container rm [id_container]
>   docker image rm [name ou id image]

15. Comando utilizando commit --> [Não recomendado, não pode ser vercioando.]
- O docker commit serve para gerar uma imagem a partir de outro container, por isso não foi necessário instalar o curl novamente pois o mesmo já 
havia sido instalado no container a qual foi criado.
>   docker commit [id_container] kubedevio/ubuntu-curl:commit

16. Criar Imagem docker com DockerFile
>   Item 1
>   docker build -t kubedevio/ubuntu-curl:build . 
- Por default ele já vai procurar o arquivo [DockerFile] tirando a necessidade de menciona-lo na criação

17. Comando para ver historico da imagem.
>   docker image history kubedevio/ubuntu-curl:commit

18. Comando para limpar imagens não utilizadas
>   docker image prune

19. Comando para executar operação dentro do container sem entra no mesmo
>   docker container run -it kubedevio/ubuntu-curl:build curl https://www.google.com

20. Comando para criar uma nova tag
- Serve também para criar uma nova imagem alterando o nome/repositorio a partir de outra imagem
>   docker tag kubedevio/api-conversao:v1 kubedevio/api-conversao:latest

21. Comando para remover todos os container - O Docker fornece um único comando que irá limpar quaisquer recursos — imagens, contêineres, volumes, e redes — que estão pendentes (não associados a um contêiner):
>   docker system prune 

22. Comando para remover todas as imagens
>   docker rmi $(docker images -a -q)

### Boas práticas Imagem Docker
- Imagem oficias
- Utilize sempre as tag de versão
- Utilize layers a seu favor
- Utilize .dockerignore

### Multistagebuild
- Utilizado para otimizar a imagem, separando as imagens em processo de build e de execução
- Ex com aplicação [GO]
-
*Primeira Imagem faz o processo de build*
> 	FROM golang:1.7.3 as build
	WORKDIR /build
	COPY main.go .
	RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .
	CMD ["./main"]

*Segunda Imagem faz o processo de execução apontado para a imagem com o build*
>	FROM alpine:latest
	WORKDIR /app
	COPY --from=build /build/main .
	CMD [ "./main" ]
	
- Otimização da imagem sem e com multistage
- Sem -> imagem com 674MB
- Com -> imagem com 7.21MB	


### Principais comandos Dockerfile
- FROM        -> Inicializa o build de uma imagem a partir de uma image base.
- RUN         -> Executa um comando
- LABEL       -> Adiciona metadados a imagem
- CMD         -> Define o comando e ou os parâmetros para executação do container
- EXPOSE      -> Define que o container precisa export a porta em questão
- ENV         -> Define variaveis de ambiente
- ADD         -> Copia arquivos ou diretório ou arquivos remotos e adiciona ao sistema de arquivos da imagem [diferencial, pode trablhar com zipados]
- COPY        -> Copia arquivos ou diretórios e adiciona ao sistema de arquivos da imagem
- ENTRYPOINT  -> Informa qual comando será executado quando um container for iniciado utilizando esta imagem, diferentemente do CMD, o ENTRYPOINT não é sobrescrito, isso quer dizer que este comando será sempre executado
- VOLUME      -> Mapeia um diretório do host para ser acessível pelo container
- WORKDIR     -> Define qual será o diretório de trabalho (lugar onde serão copiados os arquivos, e criadas novas pastas)
- ONBUILD     -> Define algumas instruções que podem ser realizadas quando alguma determinada ação for executada, é basicamente como uma trigger.

### Imagem x Container
- Imagem -> um processo que será executado, um pacote
- Container -> É um projeto criado, um processo
- Identificando Imagem Docker -> EX: kubedevio/api-conversao:v1
 - kubedevio -> namespace
 - api-conversao -> Repositório
 - v1 -> tag
 
[Obs: Quando a imagem não tem o namespace, isso significa que é uma imagem oficial do Docker.]

### Diferenças entre [DockerFile x Commit]
o primeiro trabalhar por camandas a cada linha de comando escrita no [dockerFile] e armazena em um cache, o que possibilita um ganho no futuro.
e em caso de mudança apenas o que foi incluido novo que será instalado
O que não ocorre quando utilizamos o [commit]

#### Forma de criar imagens Docker
- docker commit -> *Não recomendado* item 15
- dockerfiles -> item 16

## Azure Container Registry
- Semelhante ao Docker hub
- portal Azure - Create container registry - configurações - [PAGO]
- Habilitar Acces key para utilizar o client docker cli
- Autenticar
 - docker login url (Passada na tela de acces key)
 - username e password
- push para subir a imagem da mesma forma como foi no docker hub


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

### Pods (Pasta exercicios/modulo-11)
1. Para descobrir qual versão do apiVersion, verifica a linha de baixo no retorno do comando, como não informar será utilizado a V1
- Observar o tipo do objeto que está utilizando e esxecutar o comando abaixo para recuperar a versão coluna APIGROUP 
- Ex: Pod -> não tem nada nessa coluna, portanto a versão será v1
- Ex: replicasets -> esse já tem uma informação nessa coluna portanto a versão ficar dessa forma apps/v1
>    kubectl api-resources

2. Criar o cluster no kubernates
>  kubectl apply -f arquivo.yaml

3. Sabe mais detalhes de um objeto
>  kubectl describe pod [nomepod]

4. Bind da porta local com a porta do pod (Semelhante ao processo do docker)
>  kubectl port-forward pod/meuprimeiropod 8080:80    

### NUNCA USE O PODE SOZINHO, O MESMO NÃO TEM RESILÊNCIA/ESCALABILIDADE SE POR VENTURA CAIR OU SER DELETADO ERRONÊAMENTE ELE NÃO CRIARÁ UM NOVO AUTOMATICAMENTE.

#### Label -> Elemento chave/valor juntos com o metadados organização do projeto, utilizado para seleção 
- É possível seleciona-los por itens descrito na labels
>  kubectl get pods -l versao=verde
- É possível realizar deletes passando as labels com filtros
>  kubectl delete pod -l versao=azul

### ReplicaSets - Antigo -> Replication Controller
- Garante as replicas
- RESILÊNCIA
- Não é possivel atualizar os pods utilizando o replicaSet
- Não gerencia as versões

### Deployment
- Um objeto 
- Gerenciar os replicaSets
- Processo de entrega


#### Visão GERAL
POD -> REPLICASET -> DEPLOYMENT
Seguindo essa ordem de gerenciamento
- O deploymenet guarda os replicaset para gerenciar as versões a cada versão nova startada ele vai criar um novo replicaset
*Pod trabalha sozinho
*ReplicaSets gerencia os pods --> e nesse processo ele automaticament cria os Pod
*Deployment gerencia os replicaSet --> da mesma for nesse processo ele cria as replicaset

> kubectl get pods
> kubectl get replicaset
> kubectl get deployment

Obs: Para deletar o deployment
> kubectl delete deployment [name]

- Ver as versões criadas
> kubectl rollout history deployment meuprimeirodeployment

- Voltar uma versão
> kubectl rollout undo deployment meuprimeirodeployment

- Atualizar imagem via comando, observe que a tag *meucontainer* é a mesma informada no arquivo yaml (exercicio/modulo-11)
> kubectl set image deployment meuprimeirodeployment meucontainer=kubedevio/nginx-color:green
  
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


### SERVICE
#### Cluster IP
- Gerar conexão dentro do cluster kubernates
- Por padrão o TYPE do service é ClusterIP não sendo necessário colocar a informação
- Utilizando esse tipo ele já faz o balanceamento automatico, alterando os pods, e ao invés ultizar o IP do pod utilizamos o name do service, 
ficando assim mais dinamico

- Comando 
> kubectl run -i --tty --image douglasjava26/ubuntu-machine ping-test --restart=Never --rm -- /bin/bash


#### NodePort
- Acesso externo
- Ranche de ports -> 30.000 até 32.767
- Ao criar um service desse tipo ele vai externalizar os pods fazendo o bind da porta internar para uma porta externa utilizando esse ranche
- Quando for usar internamente ainda utiliza o name do service e a porta do service > kubectl get services -o wide
- Quando for usar externamente  utilizar o IP do multipass ou microk8s


#### LoadBalancer
- Também gerar um IP externo
- Provedor de nuvem
- serviço cloud
- Não funciona sem provedor de nuvem - observar tag [EXTENAL-IP pending]

-- para ver todos os processos pods/replicaset/deployment/service
> kubectl get all

#### ExternalName
- Serve como se fosse redirecionamento de páginas web, 
na tag spec.externalName informo o link externo para acessar e no metadata.name o nome que quero utilizar para acessar esse caminho externo

[OBS: sempre ficar atento ao nome passado selector o mesmo tem que ser passado nos dois arquivos deployment e service .yaml]

-- service.yaml
> spec:
	selector:
		app: api

-- deployment.yaml	
> spec:
	selector:
		matchLabels:
			app: api






## Endpoints
- Coleção de todos os Pods que são vinculados com o services.
- São criados de forma automatica

> kubectl get enpoints

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