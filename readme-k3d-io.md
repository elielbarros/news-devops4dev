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