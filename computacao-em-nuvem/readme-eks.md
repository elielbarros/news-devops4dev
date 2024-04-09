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

Passos para criar a rede
- Na busca do console, busque por "CloudFormation"
- Clique no botão "Create Stack"
- Consulte https://github.com/KubeDev/devops4devs-01
- Copie a URL do template do CloudFormation
- Cole no campo "Amazon S3 URL"
- Next, no campo "Stack Name", coloque: eks-net-study e clique em Next
- Em "Configure stack options" deixe as configurações como estão e continue 
  para a proxima pagina