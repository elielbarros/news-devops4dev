Passos para criar um cluster kubernetes com EKS
- Criação de Roles, um para EKS gerenciar os Clusters e outro para 
  gerenciar os Workers Nodes
1. Criar role para cluster
   - Acesse IAM > Roles > Create Role
   - Selecione a opção AWS Service
   - Em "Service or use case", busque por EKS
   - Selecione a opção EKS - Cluster
   - Deixe a pagina de "Add Permissions" apenas com a permissao "AmazonEKSClusterPolicy"
   - Na pagina "Name, Review and create" no campo "Role Name", use o nome 
     eksClusterRole
2. Criar role para EC2
   - Acesse IAM > Roles > Create Role
   - Seleciona a opção AWS Service
   - Em "Service or use case", busque por EC2
   - Selecione a opção EC2
   - Na pagina "Add permissions", busque por "AmazonEc2ContainerRegistryReadOnly"
   - Selecione essa opção
   - Adicione tambem a opção "AmazonEKS_CNI_Policy"
   - Adiciona tambem a opção "AmazonEKSWorkerNodePolicy"
   - ec2WorkerNode

Passos para criar a rede
- Na busca do console, busque por "CloudFormation"
- Clique no botão "Create Stack"
- Consulte https://github.com/KubeDev/devops4devs-01
- Copie a URL do template do CloudFormation
- Cole no campo "Amazon S3 URL"
- Next, no campo "Stack Name", coloque: eks-net-study e clique em Next
- Em "Configure stack options" deixe as configurações como estão e continue 
  para a proxima pagina

Criação do Cluster Kubernetes
- Na busca do console, busque por "Elastic Kubernetes Service"
- Clique em "Create Cluster"
- Dê um nome ao seu cluster, "news-eks"
- Escolha uma versão Kubernetes
- Defina uma role para o Cluster e deixe o restante dos itens como default
- Na pagina "Specify networking", no campo "VPC", escolha a subnet criada
- No campo Security Groups, escolha a opção criada também
- Em "Cluster endpoint access", escolha a opção "Public and Private"
- Na pagina "Configure observability" deixe tudo como padrão
- Na pagina "Select add-ons" deixe as opções selecionadas menos a opção 
  "GuardDuty"
- Na pagina "Configure selected add-ons settings" deixe tudo como padrao
- Na pagina "Review and create" apenas clique no botão "Create"
- O processo de criação do cluster leva cerca de 15 minutos

Utilize o AWS CLI para o Cluster da AWS
- Execute o comando: ```aws eks update-kubeconfig --name <nome-cluster-kubernetes-criado-aws>```
- Exemplo: ```aws eks update-kubeconfig --name news-eks```
- Será necessário atualizar o Manifesto (deploy.yaml), mudando o tipo de 
  porta de NodePort para LoadBalancer

Crie um "Node Group"
- Acesse a pagina EKS (Elastic Kubernetes Service)
- Clique no EKS criado (atualmente, news-eks)
- Acesse a aba "Compute"
- Em "Node Groups", clique no botão "Add node group"
- Na pagina "Configure node group", no campo "Name", coloque "default"
- No campo "Node IAM role", coloque a role criada "ec2WorkerNode", depois 
  "next"
- Na pagina "Set compute and scaling configuration"
  - No campo "Instance types", busque e escolha a opção "t3.medium"
  - No campo "Minimum size" escolha 1
  - No campo "Maximum size" escolha 4
  - NEXT
- Na pagina "Specify networking", como boa pratica, os "Worker Node" serão 
  criadas apenas em subnets privadas, remova as publicas
- NEXT
- CREATE
- 01:48:37