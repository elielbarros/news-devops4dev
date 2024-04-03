O que é o K3D?
- K3d é uma ferramenta de linha de comando que facilita a execução de clusters Kubernetes em um ambiente local, utilizando Docker como o mecanismo de contêineres subjacente. Em resumo, o K3d é uma maneira conveniente de criar e gerenciar clusters Kubernetes de desenvolvimento, testes e aprendizado em sua máquina local. Ele é útil para desenvolvedores que desejam experimentar, testar e desenvolver aplicativos Kubernetes sem precisar configurar um ambiente completo de produção.

Como instalar o K3D?
- https://k3d.io/v5.6.0/#installation


O que é o KUBECTL?
- O `kubectl` é uma ferramenta de linha de comando usada para interagir com clusters Kubernetes. Ele permite que os usuários executem várias operações em clusters Kubernetes, como criar, modificar e excluir recursos, como pods, serviços, deployments e ingressos. Com o `kubectl`, os usuários podem implantar aplicativos, inspecionar e gerenciar o estado dos clusters, acessar logs de contêineres e executar outras tarefas de administração. Essa ferramenta é essencial para administradores de sistemas, desenvolvedores e operadores que trabalham com Kubernetes.

Como instalar o KUBECTL?
- https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/


Como criar um cluster Kubernetes com K3D?
- Execute o comando ```k3d cluster create```
- Execute o comando ```docker container ls``` e será possível observar os containers que foram criados para o cluster.
- São eles: k3d-tools; k3d-proxy; k3s
- k3d-tools: ferramental auxiliar para criar o cluster
- k3d-proxy: responsavel pelo balanceamento de cargas dos 'nodes' (Loadbalancer)
- k3s: server

Como criar um cluster Kubernetes nomeado?
- Execute o comando: ```k3d cluster create <NOME-CLUSTER>```
- Para criar um cluster com numero maior de servers/control planes, execute o comando a seguir:
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
- Os containers que estão dentro do POD, podem compartilhar o endereço IP e o sistema de arquivos
- Não é ideal executar todos os containers de uma aplicação no POD, pois o que é escalado não é o container, é o POD. Com isso, quando acontecer o escalonamento, todos os containers dentro do POD serao escalonados juntos. Replica o ambiente todo, sem necessidade.
- O ideal é colocar cada container em um POD especifico para escalar individualmente
- Pode acontecer de ter dois containers no mesmo POD, um container principal, no caso um front e um serviço secundario responsavel por coleta de log
- O POD sozinho nao é capaz de escalonar-se, é necessário um controlador acima do POD para gerenciar as replicas e sua respectiva resiliencia, é necessário utilizar o ReplicaSet

O que é o ReplicaSet?
- Elemento que cuida da execução de todos os PODs
- Garante que a quantidade de replicas especificadas a nivel de codigo esteja em execução no ambiente
- Caso um POD seja eliminado por N motivos, o ReplicaSet cria um novo POD, assim como elimina um POD se a necessidade do codigo mudar
- O ReplicaSet vai garantir escalabilidade e resiliencia dos PODs mas não vai garantir a facilidade na troca de versao da aplicação, trocar o pneu do carro com ele andando. Pra isso entra outro controlador chamado Deployment

O que é Deployment?
- Controlador responsavel por gerenciar os ReplicaSets disponiveis.
- ![Deployment.png](.static%2FDeployment.png)
- Caso seja alterado as especificações do codigo, por exemplo, a mudança de versao da aplicação, o Deploymente será responsável por criar um novo ReplicaSet com a versao respectiva, destruir os PODs da versao antiga e construir novos PODs no novo ReplicaSet
- O Deployment não remove o ReplicaSet anterior para caso aconteça algum problema no ReplicaSet novo, seja possivel retornar para a versao antiga com facilidade
- Como se faz para conectar o Deployment ao ReplicaSet e vice versa? Com Labels e Selectors

O que é Label e Selector?
- Label é um elemento de Chave-Valor que marca um objeto no Cluster Kubernetes com informações importantes
- ![LabelSelector.png](.static%2FLabelSelector.png)
- O Selector será responsável por selecionar os objetos atraves de suas Labels, utilizando as mesmas chaves e valores