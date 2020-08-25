### Anotações / Comandos

------------------------------------------
ANOTAÇÕES ESTUDOS KUBERNATES/DOCKER
------------------------------------------

comandos docker

docker build -t douglasjava26/curso:v1 -f .\src\Dockerfile .\src\
	
**Explicação**

Criar a imagem
- t -> criação de tag, o nome antes da '/' é o repositório no Docker HUB 
- f -> apontamento para o arquivo do Dockerfile


docker push douglasjava26/curso:v1
subir a imagem para o hub

comando kubernates

SUBINDO BANCO
kubectl apply -f .\k8s\mongodb\service.yaml
kubectl apply -f .\k8s\mongodb\deployment.yaml

SUBINDO API
kubectl apply -f .\k8s\api\service.yaml
kubectl apply -f .\k8s\api\deployment.yaml

**Explicação**

Fazer deploy dos elementos service/deployment aplicando-os
-f -> apontamento para o arquivo de service

COMO SEI QUE FOI REALMENTE FEITO O DEPLOY 
kubectl get deployments

COMO VEJO OS PODS
kubectl get pods

COMO RECUPERAR MEU SERVIÇO QUE FOI STARTADO 
kubectl get services

COMO ESCALAR UM DEPLOYMENT - UM SERVIÇO 
kubectl scale deployment api-deployment --replicas=10

COMO REMOVER UM POD 
kubectl delete pods api-deployment-749d78579f-5f4mj 

QUANTOS SERVIDORES ESTÃO SENDO EXECUTADOS
kubectl get nodes   

MAIS ESPECIFICO
kubectl get pods -o wide

ATUALIZAR IMAGEM DO DEPLOYMENT 
kubectl set image deployment api-deployment api=douglasjava26/curso:v2

CONFIRMA QUE A IMAGEM FOI ALTERADA
kubectl describe pod 


VER METRICAS DE CADA POD
kubectl top pod --> Obs:: é ncessário instalar o metricsservice  --> kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.7/components.yaml


VER QUANTAS VERÕES EXISTEM 
kubectl rollout history deployment api-deployment

VOLTAR VERSÃO 
kubectl rollout undo deployment api-deployment

FERRAMENTA DE DASHBOARD
Octant 
Instalção --> choco install octant --confirm  (abrir cmd como ADM)


REMOVER SERVICES DO KUBERNATES

kubectl get service -o wide

kubectl delete svc <YourServiceName>

docker run -p 8080:8080 douglasjava26/blockchain:v1

VER LOGS
kubectl logs 'nome do pod'


Monitoramento
kubectl get hpa

CONFIGURAÇÂO RESOURCES -> deployment
Configuração realizada para execução do container, requisitar um padrão minimo e um limite para cira outra replica

Requests -> é a configuração minima
Limits -> é a a configuração máxima


o que acontece quando estoura o container se não tiver configurado.
com essa configuração automaticamente ele vai criando mais replicas dependendo das requisições realizadas
com a caraga voltando ao normal ele mesmo vai removendo as replicas automaticamente, e nesse processo
a um aviso para o servidor quando uma replica estiver sendo deletado, para ela retirar da fila de loadbalance




----------------------------------------------
-           COMANDOS MULTIPASS               -
----------------------------------------------

---> Criação de máquinas virtuais

> multipass launch -n k8s -c 1 -m 2gb -d 20gb
-n --> especificar o nome da vm
-c quantidade de nucleos
-m quantidade de memória
-d quantidade de disco

> multipass shell k8s
- acessar internamente a VM

> multipass exec k8s -- comando linux
-- comando de fora da VM

> multipass mount C:Users\Marques k8s:/externo
-- montar diretório da máquina física dentro da VM

> multipass umount k8s
-- remover mapeamento

> multipass info k8s
-- recuperar informções sobre as VM

> multipass delete k8s
-- remover pacialmente

> multipass recover k8s
-- recuperar VM de delete pacialmente

> multipass delete k8s
> multipass purge
-- delete completo


---> POSSIVEL ERRO NO WINDOWS 
Falha ao criar VM - timed out 

*ERRO*
launch failed: The following errors occurred:
unassuming-tailorbird: timed out waiting for response

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