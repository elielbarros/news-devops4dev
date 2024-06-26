O que é o K3D?
- K3d é uma ferramenta de linha de comando que facilita a execução de 
  clusters Kubernetes em um ambiente local, utilizando Docker como o 
  mecanismo de contêineres subjacente. Em resumo, o K3d é uma maneira 
  conveniente de criar e gerenciar clusters Kubernetes de desenvolvimento, 
  testes e aprendizado em sua máquina local. Ele é útil para 
  desenvolvedores que desejam experimentar, testar e desenvolver 
  aplicativos Kubernetes sem precisar configurar um ambiente completo de 
  produção.

Como instalar o K3D?
- https://k3d.io/v5.6.0/#installation


O que é o KUBECTL?
- O `kubectl` é uma ferramenta de linha de comando usada para interagir com 
  clusters Kubernetes. Ele permite que os usuários executem várias 
  operações em clusters Kubernetes, como criar, modificar e excluir 
  recursos, como pods, serviços, deployments e ingressos. Com o `kubectl`, 
  os usuários podem implantar aplicativos, inspecionar e gerenciar o estado 
  dos clusters, acessar logs de contêineres e executar outras tarefas de 
  administração. Essa ferramenta é essencial para administradores de 
  sistemas, desenvolvedores e operadores que trabalham com Kubernetes.

Como instalar o KUBECTL?
- https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/


Como criar um cluster Kubernetes com K3D?
- Execute o comando ```k3d cluster create```
- Execute o comando ```docker container ls``` e será possível observar os 
  containers que foram criados para o cluster.
- São eles: k3d-tools; k3d-proxy; k3s
- k3d-tools: ferramental auxiliar para criar o cluster
- k3d-proxy: responsavel pelo balanceamento de cargas dos 'nodes' (Loadbalancer)
- k3s: server

Como criar um cluster Kubernetes nomeado?
- Execute o comando: ```k3d cluster create <NOME-CLUSTER>```
- Para criar um cluster com numero maior de servers/control planes, execute 
  o comando a seguir:
- ```k3d cluster create <NOME-CLUSTER>  --servers <qtd> --agents <qtd>```
- Exemplo: ```k3d cluster create meucluster --servers 3 --agents 3```

Como listar os 'nodes' criados ao criar o cluster?
- Execute o comando ```kubectl get nodes```

Como listar os clusters kubernetes criados?
- Execute o comando ```k3d cluster list```

Como remover um cluster Kubernetes?
- Faça a listagem dos clusters criados com o comando ```k3d cluster list```
- E delete utilizando o nome do cluster com o comando a seguir:
- ```k3d cluster delete <nome-do-cluster>```

NOTE: CASO O CLUSTER APRESENTE ALGUMA FALHA, DESTRUA E CRIE-O NOVAMENTE!

O que é POD ?
- Menor objeto do cluster Kubernetes
- Nele que é executado os containers
- No POD pode ter mais que um container
- Os containers que estão dentro do POD, podem compartilhar o endereço IP e 
  o sistema de arquivos
- Não é ideal executar todos os containers de uma aplicação no POD, pois o 
  que é escalado não é o container, é o POD. Com isso, quando acontecer o 
  escalonamento, todos os containers dentro do POD serao escalonados juntos.
  Replica o ambiente todo, sem necessidade.
- O ideal é colocar cada container em um POD especifico para escalar 
  individualmente
- Pode acontecer de ter dois containers no mesmo POD, um container 
  principal, no caso um front e um serviço secundario responsavel por 
  coleta de log
- O POD sozinho nao é capaz de escalonar-se, é necessário um controlador 
  acima do POD para gerenciar as replicas e sua respectiva resiliencia, é 
  necessário utilizar o ReplicaSet

O que é o ReplicaSet?
- Elemento que cuida da execução de todos os PODs
- Garante que a quantidade de replicas especificadas a nivel de codigo 
  esteja em execução no ambiente
- Caso um POD seja eliminado por N motivos, o ReplicaSet cria um novo POD, 
  assim como elimina um POD se a necessidade do codigo mudar
- O ReplicaSet vai garantir escalabilidade e resiliencia dos PODs mas não 
  vai garantir a facilidade na troca de versao da aplicação, trocar o pneu 
  do carro com ele andando. Pra isso entra outro controlador chamado Deployment

O que é Deployment?
- Controlador responsavel por gerenciar os ReplicaSets disponiveis.
- ![Deployment.png](.static%2FDeployment.png)
- Caso seja alterado as especificações do codigo, por exemplo, a mudança de 
  versao da aplicação, o Deploymente será responsável por criar um novo 
  ReplicaSet com a versao respectiva, destruir os PODs da versao antiga e 
  construir novos PODs no novo ReplicaSet
- O Deployment não remove o ReplicaSet anterior para caso aconteça algum 
  problema no ReplicaSet novo, seja possivel retornar para a versao antiga 
  com facilidade
- Como se faz para conectar o Deployment ao ReplicaSet e vice versa? Com 
  Labels e Selectors

O que é Label e Selector?
- Label é um elemento de Chave-Valor que marca um objeto no Cluster 
  Kubernetes com informações importantes
- ![LabelSelector.png](.static%2FLabelSelector.png)
- O Selector será responsável por selecionar os objetos atraves de suas 
  Labels, utilizando as mesmas chaves e valores

O que é o Service Discovery?
- O Service Discovery serve para realizar a comunicação entre uma 
  aplicação B e uma aplicação A escalonada.
- Ele é responsável por receber a requisição da aplicação B e encaminhar a 
  requisição para uma das aplicações A disponível no momento.
- Então o Service Discovery, vai ajudar com o balanceamento de carga entre 
  as replicas
- ![ServiceDiscovery.png](.static%2FServiceDiscovery.png)

Existem três tipos de Service
- ClusterIP: (Visto no Service Discovery) Ponto unico de comunicação com os 
  PODs, mas internamente no Cluster Kubernetes, ou seja, se for desejado 
  realizar o acesso internamente dos PODs, é com o Service do tipo 
  ClusterIP que será utilizado, se for desejado fazer o acesso externamente,
  não será possivel. 
- NodePort: Cria um acesso externo para os PODs. É possível acessar de fora 
  do Cluster Kubernetes, os PODs atraves do Service NodePort
  - Expoe a aplicação para acesso externo.
  - Utiliza um range de porta especifico que o Kubernetes libera. Fica 
    entre 30000 e 32767
  - ![NodePort.png](.static%2FNodePort.png)
  - Para acessar a aplicação, é suficiente utilizar qualquer ip das 
    maquinas que façam parte do cluster kubernetes e a porta escolhida
- LoadBalancer: Cria um LoadBalancer do code provider e expor através de um 
  ip publico. Cria um IP especifico para poder acessar o service.
  - Esse service em um ambiente local/on premise não vai funcionar pq nao 
    tem conectividade com o cloud provider para criar o LoadBalancer (o ip 
    publico). Resumindo, ambiente local/OnPremise vai funcionar com o NodePort

    
Como criar esses objetos citados anteriormente dentro do cluster Kubernetes?
- Utilize um 'Arquivo Manifesto'
- O 'Arquivo Manifesto', é um arquivo YAML que especificará todos os objetos 
  do cluster Kubernetes
- No arquivo manifesto é necessário alguns campos OBRIGATORIOS, são eles: 
  apiVersion, kind, metadata, spec

Como listar a lista de recursos disponiveis da API do Kubernetes?
- Execute o comando: ```kubectl api-resources```
- Lá será possível visualizar o nome dos objetos e seus Short Names, alem 
  de suas respectivas versoes, namespaced e kind

Como aplicar as definições do 'Arquivo Manifesto' do POD no cluster Kubernetes?
- Execute o seguinte comando: ```kubectl apply -f <diretorio/nome-arquivo.
  yaml>```
- Exemplo: ```kubectl apply -f k8s/deploy.yaml```
- Após isso, execute o comando ```kubectl get pods``` para verificar o POD 
  criado

Como aplicar as definições do 'Arquivo Manifesto' do Service no cluster 
Kubernetes?
- Utilizando o mesmo arquivo criado, inicialmente para as definições do POD,
  ao final do arquivo coloque --- para separar o objeto POD do service e em 
  seguida comece as definições para o Service
- Defina quais PODs serão expostos através do Service, utilizando o selector
- Defina a porta que o POD será exposto utilizando ports
- Execute o comando para aplicar as definições de Service:
- ```kubectl apply -f <caminho-para-arquivo.yaml>```
- Exemplo: ```kubectl apply -f k8s/deploy.yaml```
- Execute o comando ```kubectl get service``` para listar o service criado
- Para listar o deployment execute o seguinte comando:
- ```kubectl get deployment```
- Para listar o ReplicaSet execute o seguinte comando:
- ```kubectl get replicaset```
- Para listar todos os Objetos do Kubernetes execute o seguinte comando:
- ```kubectl get all```
- Faça a deleção dos PODs com o seguinte comando e depois execute o get all 
  e será possível observar que o ReplicaSet ira criar novos PODs
- ```kubectl delete pod <nome-pod>```

Como acessar o POD na maquina local, para testar o POD criado?
- No exemplo atual, o Service foi criado com ClusterIP e só pode ser 
  acessado apos exposição da porta utilizando o seguinte comando:
- ```kubectl port-forward <nome-pod> <porta-local>:<porta-pod>```
- Exemplo: ```kubectl port-forward pod/postgre-etc-etc 5432:5432```

Como acessar o Service na maquina local, para testar o POD?
- No exemplo atual, o Service foi criado com ClusterIP e só pode ser 
  acessado apos exposição da porta utilizando o seguinte comando:
- ```kubectl port-forward <nome-service> <porta-local>:<porta-pod>```
- Exemplo: ```kubectl port-forward service/postgre 5432:5432```

Como expor a porta do serviço web sem a necessidade de usar o port-forward?
- Faça a deleção do cluster criado anteriormente com o seguinte comando:
- ```k3d cluster delete meucluster```
- Recrie o cluster utilizando o parametro X no comando a seguir:
- ```k3d cluster create meucluster --servers 3 --agents 3 -p 
<porta-maquina-local>:<porta-container@loadbalancer>```
- Exemplo: ```k3d cluster create meucluster --servers 3 --agents 3 -p "30000:30000@loadbalancer"```
- No 'Arquivo Manifesto', no Service da aplicação Web, em ports, adicione a 
  chave nodePort e o valor 30000
- Em seguida execute as definições do 'Arquivo Manifesto' com o seguinte 
  comando:
- ```kubectl apply -f k8s/deploy.yaml```

Como aplicar as definições do 'Arquivo Manifesto' e assistir as 
atualizações que acontecem nos antigos objetos?
- Execute o seguinte comando:
- ```kubectl apply -f k8s/deploy.yaml && watch 'kubectl get pod'```

Como ver o historico de mudança do ReplicaSet?
- Execute o seguinte comando:
- ```kubectl rollout history deployment <nome-do-service>```
- Exemplo: ```kubectl rollout history deployment web```

Como fazer o rollback para a versao antiga do ReplicaSet?
- Execute o seguinte comando:
- ```kubectl rollout undo deployment <nome-do-service>```
- Exemplo: ```kubectl rollout undo deployment web && watch 'kubectl get pod'```