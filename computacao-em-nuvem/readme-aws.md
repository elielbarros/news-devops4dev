Regioes da AWS
- https://aws.amazon.com/about-aws/global-infrastructure/regions_az/

Free trial AWS
- https://aws.amazon.com/free/?gclid=Cj0KCQjw5cOwBhCiARIsAJ5njuaqLEBxEl170tZ8x5AytrtgGU4ODO0M3QvipHCdRA7f-DflKcxnRbwaAtlUEALw_wcB&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=categories%23compute&trk=d0b462ed-a9ff-4714-8a75-634758c49d4c&sc_channel=ps&ef_id=Cj0KCQjw5cOwBhCiARIsAJ5njuaqLEBxEl170tZ8x5AytrtgGU4ODO0M3QvipHCdRA7f-DflKcxnRbwaAtlUEALw_wcB:G:s&s_kwcid=AL!4422!3!531081610020!e!!g!!aws%20free%20cloud%20server!12024810921!121376982172

AWS CLI
- Linux Installation: 
- https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html#cliv2-linux-install

Primeiros passos
- No painel da AWS, mudar a região para N. Virginia, pois é a região menos 
  custosa
- Engrenagem > More User Settings > Localization and default Region > Edit
  - Default Region > US East (N. Virginia) us-east-1
- Criar um usuario novo para não usar o usuario root

Identity and Access Management (IAM)
- Policies:
  - Politicas de acesso ao usuario
  - Define o que o usuario, grupo ou role, pode acessar
  - Existem politicas costumizadas ou padrão
- Users:
  - Usuario que tem acesso a conta, gerenciamento de cloud e etc
- User Groups:
  - Grupo onde o usuario é adicionado
  - No grupo, pode se vincular politicas de acesso
  - O usuario que pertencer a esse grupo seguirá as politicas de acesso que 
    o grupo tem

User Groups
- Acesse IAM > Access Management > User Groups > Create Group
- Adicione um nome ao grupo no campo 'User group name'
- Escolha as Policies do grupo que pretende criar
- Será criado um grupo com nome 'administrador' e com a police padrao 'AdministratorAccess'

Users
- Acesse IAM > Access Management > Users > Create User
- Adicione um nome ao usuario no campo 'User name'
- Se desejar que o usuario tenha acesso a pagina de console da aws, clique 
  na opção 'Provide user access to the AWS Management Console - optional'
- Clique em 'I want to create an IAM user'
- Clique em Custom password e insira uma senha
- Se desejar que o usuario troque a senha ao logar marque a opção 'Users must create a new password at next sign-in - Recommended'
- No momento, não será marcado essa opção
- Na proxima pagina, adicione o usuario ao grupo que ele pertence.
- É possível também copiar permissões de outro usuario ou colocar 
  'policies' especificar para o usuario, mas nesse momento, o usuario 
  criado será colocado no grupo criado 'administrador'
- Ao final é possível fazer o download com as informações do usuario criado.

Roles
- Role é vinculado a um serviço da AWS que permite acesso a outro serviço AWS
- Por exemplo, um serviço EC2 com Jenkins que precisa fazer registros em um 
  serviço ECR


Como configurar a ferramenta de linha de comando (CLI)?
- Acesse o usuario criado
- Acesse o IAM > Users > Access Keys > Create Access Key
- Escolha a opção Command Line Interface (CLI)
- Preencha a opção 'I understand the above recommendation and want to proceed to create an access key.'
- No campo Description tag value, insira um valor que descreva. Será usado 
  'CLI'
- Abra o terminal e execute o comando ```aws configure```
- Copie o Access Key criado e cole no terminal
- Copie o Secret access key e cole no terminal
- Coloque a região us-east-1 relativa a regiao N. Virginia
- Em 'Default output format [None]' apenas entre com o enter

Feito isso, será utilizado o serviço da AWS S3 para fazer o STORAGE da 
aplicação 'news'
- Acesse o 'Console Home'
- Busque por S3

O que é o S3?
- Simple Storage Service (S3)
- Serviço para armazenamento de objetos da AWS
- Pode ser utilizado para armazenamento de Backup, Arquivos em geral
- Pode ser utilizado para compartilhamento de arquivos entre diversos usuarios
- Pode fazer o versionamento de arquivos
- Pode trabalhar com multiplas regioes
- Pode ter multiplos endpoints para cada região
- Garantindo alta velocidade independente da regiao em que está sendo 
  trabalhado
- Controle de acesso de arquivos (segurança de arquivos)
  - O que é Bucket?
    - Onde serão armazenados os objetos (arquivos)
  - Como criar um Bucket?
    - Busque por S3 > Create bucket
    - Escolha momentaneamente a opção 'General porpuse'
    - Em 'Bucket Name' coloque um nome unico
    - Escolha momentaneamente a opção 'ACLs enabled', já que será 
      implementado no codigo e habilitado permissionamento baseado em ACL
    - Escolha a opçao 'Bucket owner preferred' em 'Object Ownership'
    - Já que a aplicação tem que acessar o S3 publicamente, a opção 'Block 
      all public access' ficará desmarcada.
    - Marque a opção 'I acknowledge that the current settings might result in this bucket and the objects within becoming public.'
    - Em 'Bucket Versioning' ficará marcado a opção 'Disable' para evitar 
      consumir mais espaço no serviço S3 e ser cobrado por isso
    - O restante das informações serão permanecidas com o default
    - Clique em 'Create Bucket'