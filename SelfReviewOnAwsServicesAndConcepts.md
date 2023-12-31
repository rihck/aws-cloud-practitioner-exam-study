## EC2 Instances
É uma maquina virtual que tu aluga na AWS. Eles virtualizam uma maquina virtual num servidor fisico e a responsabilidade de administrar a máquina é sua. Como patch, manter segurança, firewall, etc.

### Tipos de preços de EC2
- OnDemand: Você paga o que você usa sob demanda. Não tem comprometimento em usar um valor especifico mas por isso também não tem descontos
- Reserved Instances: Você se compromete em contratos de 1 ou 3 anos pra usar e por isso tem um desconto significativo no valor
- Saving Plans: Você se compromete a usar um minimo por mês e consegue ter um desconto significativo.
- Spot instances: Até 90% de desconto mas a instancia pode ser tomada de você a qualquer hora, eles avisam 2 minutos antes. Ideal pra trabalhos que você não precisa persistir estado como batch ou processos com dados temporários.
- Dedicated..?: Quando você quer um hardware físico só pra você, por questões de compilance em que o padrão da AWS de Multi Tenancy (virtualizar várias máquinas no mesmo hardware) não funciona pra você

### Tipos de Instância
- General purpose (Balanced): Equilibrio de todos recursos
- Storage Optmized: Mais performance pra dados salvos localmente (talvez com SSD?)
- Memory Optmized: Quando precisa de muito processamento em memória, otimizado
- Compute Optimized: Quando precisa de muito processamento (talvez mais CPU)
- Acelerate Computing: Usa "aceleradores de hardware" seja lá o que isso for, usado pra coisas tipo Graphs, processamento de padrões de dados, etc.

---

## Amazon EC2 Auto Scaling
Onde basicamente você configura parametros para a AWS cuidar do escalamento e subir mais instancias automaticamente de acordo com:
- Minimo de instancias
- Num de instancias desejadas
- Maximo de instancias

---

## Elastic Load Balancing (ELB)
Basicamente uma solução para distribuir a carga entre suas instâncias (não escala tuas instancias, só distribui a carga). E a solução em si é auto escalável, ela consegue se escalar ao receber muita demanda.

---

## AMAZON SNS e SQS
Basicamente os serviços de fila e topico da AWS

- SQS: Simple Queue Service
- QNS: Simple Notification Service

---

## AWS Lambda
Solução Servless da AWS em que você deploya seu código (Função Lambda) e configura um gatilho. Você não precisa se preocupar com o servidor em si, só deployar seu código e garantir que ele rode em menos de 15 minutos porque é o tempo limite de uma função Lambda. Você paga por tempo de processamento no geral no fim do mes

--

## Amazon Elastic Container Service (ECS) / Amazon Elastic Kubernetes Service (EKS):
Outra solução Servless da AWS, basicamente pra você rodar coisas conternizadas. Em ambas vocês não precisa managear os containers. 

--

## AWS Fargate?
É usado para rodar containers sem precisar gerenciar servidores ou os cluster na AWS.


## Região e Zonas
É uma região é um local composto de várias zonas, e cada zona é um conjunto de recursos isolados na AWS.
Por exemplo, o brasil pode ter 3 regiões, ai você escolhe a Região Brasil e Zona-3 pra rodar sua aplicação.
Cada região tem um preço e você escolhe dependendo da necessidade da sua aplicação (mais próxima do usuário / mais barata ou por questões de compilance)


## CDN
É tipo uma cópia de dados da sua aplicação que tu coloca mais perto do seu usário, pra diminuir a latencia.

### Amazon CloudFront
É a solução da amazon que implementa essa definição de CDN, você usa quando quer ter essa cópias locais de dados da aplicação em lugares específicos, mais perto do usuário pra diminuir a latencia

### Edge Zones
A amazon chama os lugares que hospedam essas cópias de Edge Zones, são tipos zonas não convencionais que eles usam para guardar esses dados de CDN e também outras coisas tipo o Amazon Route 53.

## Amazon Route 53
É o serviço de DNS da Amazon, que mapeia teu endereço DNS pra um IP.
E você também pode deixar esse routeamento smart configurando baseado em por exemplo:
- Geolocalização
- RoundRobin / Weigth Round Robin
- Latencia

## AWS Outposts
É basicamente instalar os recursos da AWS num servidor físico teu, quando tu quer por exemplo colocar teu App numa região que a AWS não cobre ou quer só usar teus recursos físicos mesmo.

---

## Formas de Interagir com a AWS
- Browser: Ir no navegador através de UI e gerenciar as coisas
- CLI: Interagir através de command line, tipo APIs REST
- SDK: Importar uma LIB no teu código pra interagir com os recursos
- AWS CloudFormation: Tu cria um arquivo JSON com as configs abstraídas de quais recursos tu quer, quantas instancias, etc. E esse arquivo serve de template. Focam nesse exemplo falando: Você só se preocupa com o QUE VOCÊ QUER e não COMO FAZER.
- AWS Elastic Beanstalk: É a mesma ideia do CloudFormation mas não é através de arquivo, tu fala o que você quer, quantas instancias e ele deploya pra ti e gerencia

---

## Amazon Virtual Private Cloud (VPC)
É basicamente criar uma sessão isolada de rede na AWS e colocar seus recursos lá. Essa VPC não tem acesso exterior nenhum ao criada, ela pode ter dependendo do conector que você coloca nela que podem ser:
- Virtual Private Gateway: Só deixa entrar conexões autorizadas
- Internet Gateway: Deixar entrar conexões no geral
- AWS Direct Connect: Criar uma conexão direta física (Fibra) do seu local físico para AWS (através de parceiros na área)

### Subnets
É criar outra separação lógica dentro de uma VPC para limitar acesso. Você pode ter uma VPC com subnets e controlar permissões a nível de subnet.

### Network ACL (Network Access Control List)
Basicamente uma configuração a nivel de rede que você configura acessos, por padrão tudo é permitido e você começa a configurar as excessões.
Vale lembrar que ele é stateless, não guarda estado, então é a request é validada na entrada e saida, de forma que pode acontecer da request poder entrar mas não poder sair dependendo do tipo de configuração

### Security Group
Uma configuração a nível de instância EC2, por padrão vem definido para negar todas requisições e você precisa habilitar os acessos.
Ela é stateful, ou seja, mantém estado e não valida as informações na saída, somente entrada. Basicamente tudo que entra, sai.

---

## Instance Volume
Basicamente um DB a nivel de instancia EC2, ele está diretamente ligado a instancia que está rodando, se a instancia for reiniciada os dados se perdem. 
Indicado para casos de processamento em batch ou que os dados podem ser recriados facilmente

## Amazon Elastic Block Store (EBS) [Block Storage]
Diferente do volume de instância, é separado da instancia EC2 mas pode ser "plugada" na instância e pode ser reutilizada sem perder os dados ao mudar de instancia/ligar/desligar.
É um armazenamento do tipo "Block", que é salvo como um todo num bloco só, ele faz backup incremental agendado.

## Amazon Simple Storage Service (Amazon S3) [Object Storage]
É um tipo de armazenamento que salva as coisas no formato de objeto, basicamente composto de Key+Objeto+Metadata. Você pode guardar qualquer coisa tipo videos, imagem, arquivos ,etc.
Tem armazenamento ilimitado no geral e cada objeto pode pesar no máximo 5 TB. O preço "por MB" dele fica mais barato conforme você usa mais.

O S3 tem diferentes tipos de tiers que você escolhe baseado em "Qual frequente você vai acessar aquele dado" E "Em quanto tempo você precisa que ele esteja disponível". Os tipos de tiers variam com base nisso e os preços também.

| Nome                                                    | Definicao                                                                                                                                                          | Frequencia Acesso a Dados  | Disponibilidade dos Dados (Velocidade para acessar quando precisa) |   |
|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|--------------------------------------------------------------------|---|
| S3                                                      | Tipo padrao                                                                                                                                                        | Alta                       | Alta                                                               |   |
| S3 Intelligent Tiering                                  | Ele move os dados de forma automatica de acordo com o uso deles, pode ir para as categorias "Acesso Frequente" e "Acesso Infrequente"                              | Depende                    | Depende                                                            |   |
| S3 Standard-Infrequent Acess (S3 Standard-IA)           | Quando dados nao sao muito acessados MAS quando necessarios precisam ser acessado rapido                                                                           | Pouca                      | Alta                                                               |   |
| S3 One-Zone Standard-Infrequent Access (S3 One Zone-IA) | Igual o Standard-Infrequent porem os dados sao armazenados numa unica zona, e isso gera economia de 20%. Desvantagem: Caso Zona seja destruida, seus dados tbm sao | Pouca                      | Alta                                                               |   |
| Amazon S3 Glacier (S3 Glacier)                          | Armazenamento com Baixo custo ideal para arquivamento, o acesso ao dado varia de minutos ate horas dependendo do tipo da categoria                                 | Depende da Categoria       | Depende da Categoria                                               |   |
| S3 Glacier Deep Archive                                 | Menor custo de todos MAS pode ser acessado de 1 ate 2 vezes por ano.                                                                                               | Baixissima (1 a 2x no ano) | Baixissima (12 a 48 horas)                                         |   |
| S3 Glacier Flexible Retrieval                           | Mais flexivel que o Deep Archive, mas mais caro. Nivel Intermediario do Glacier                                                                                    | Baixa                      | Baixa (Minutos ate 12 horas)                                       |   |
| S3 Glacier Instant Retrieval                            | Acesso rapido em mili segundos mas mais caro das 3 opcoes                                                                                                          | Baixa                      | Alta (mili-segundos)                                               |   |

---

## Amazon Elastic File System (EFS)
É parecido com o EBS (Elastic Block Storage) com a diferença que o EFS é MultiZone, ele é um serviço que pode rodar em N zonas, já o EBS roda em uma unica zona acoplada a instancia do EC2.

## Amazon Relational Database (RDS)
É um serviço que permite rodar bancos relacionais na AWS, é uma solução pra você não precisar ter uma instancia EC2 e ter que manter ela (cuidar de patching, etc). Você instala seu banco relacional, tipo MySQL e o ambiente é gerenciado pela AWS, coisas como o patching do OS e do banco relacional em si.

## Amazon Aurora
É o banco relacional da AWS, tipo um MySQL da vida. Na teoria é mais rapido que os outros bancos relacionais.

## Amazon Dynamo DB
É o DB não relacional da amazon, baseado em Key-Value, é servless e altamente escalável.

## Amazon Redshift
É um warehouse para PowerBI / Analytics, muito escalável, aguenta muito volume e deixa tu coletar dados de N bases pra manipular nesses contextos de relatórios, reports, etc.

## AWS Data Migration Service
Como o nome já diz, destinado a migração de dados para dentro/fora da AWS, galera pode usar pra: 
- Testar uma migração parcialmente, 
- Consolidar bancos de dados (pegar de varias fontes e juntar tudo num só)
- Replicação continua (tipo um Backup incremental?)

## Amazon Document DB
Baseado em Mongo DB, um document-based DB

## Amazon Neptune
É pra Graph (Grafo) e umas paradas loucas assim, tipo detecção de Fraude, etc.

## Amazon ElasticCache
É colocar uma camada de Cache em cima de bancos pra ficar mais rapido ainda. Da pra usar Redis e MemCache pra implementar

## Amazon DynamoDB Acelerator
Deixa o DynamoDB mais rapido ainda, de milisegundos pra microsegundos.

## Amazon Managed Blockchain
Como o nome já diz, usado para "Gerenciar Redes Block Chain", seja lá o que for isso.

## Amazon Quantum Ledger 
Ledger quer dizer razão, é tipo uma base de homologação, que tu pode ver o histórico completo das tuas aplicações.

---

## AWS Identity and Access Manager (IAM)
É tipo o esquema do Spring Security, onde você configura: Usuários, Roles e Permissões e da acesso a nivel de usuario, grupos ou roles.

Se baseia em criar grupos, usuários e cargos e distribuir permissões nesses diferentes níveis.
PS: Eles consideram o Approach de roles como "dar acesso temporário a algo", tipo dar uma role temporária para dar acesso e depois remover a role
PS II: Contam com a estratégia de "menos acesso possível", onde deve-se restringir o acesso ao máximo deixando só o necessário

Também existem as Policies, que é basicamente um documento que define acessos ou restrições aos recursos da AWS

## AWS Organizations
É um lugar que você gerenia as contas e recursos da AWS num unico lugar, basicamente um lugar de consolidação onde você pode verificar e gerenciar acessos a nivel de usuário, conta, grupos, recursos, etc.

### Organizational Units
É basicamente agrupar coisas (usuários, grupos, cargos) nessas organizações para consolidar de uma forma melhor e aplicar coisas a nível disso tipo: acessos, restrições a recursos, ações, etc.

## AWS Artifact
É um lugar que tem as politicas de compilance dos serviços da Amazon, cada serviço tem um compilance diferente e tudo está consolidado e pode ser consultado aqui

---

## AWS Shield
É uma solução da AWS que protege dos ataques mais comuns de DDOS, é de graça nos serviços que herdam isso.

## AWS Shield Advanced
É igual o AWS Shield mas é pago porque tem proteções mais avançadas de DDoS e permite tu configurar politicas customizadas e ter acesso a dados customizados dos ataques que aconteceram e da rede em si.

## AWS Key
Serviço de encriptação e gerenciamento de chaves da AWS

## Amazon Web Application Firewall (AWS WAF)
É um firewall pra tua aplicação que te deixa ver (e talvez filtrar) as requests chegando

## Amazon Inspector
Lembra o Veracode Scan, ele ve potencial ameaças em configurações e definições, meio que a nível da sua aplicação só, a nivel do EC2 também, de verificar por exemplo se a sua instancia tem pontos de acesso aberto

## Amazon GuardDuty
É mais completo que o Inspector, ele analisa toda a estrutura AWS da sua conta, envolve Machine Learning e visa te dar outputs de vulnerabilidade.
Ele analise como workloads, data armazenada e você também pode configurar triggers inteligentes para quando achar alguma coisa de segurança.

---

## Amazon CloudWatch
Esse sim é igual o Dynatrace, serve pra monitorar sua aplicação no geral, ver estatísticas, logs, métricas e até configurar alertas baseado nessas coisas.

## AWS CloudTrail
É um lugar que registra todas as ações realizadas nas contas e nas aplicações da AWS, uma espécie de lugar para consolidação e ver tudo que aconteceu, tipo um histórico. Tipo, quem/onde/quando fez isso (criou um usuário, mudou permissão, startou/parou um recurso, etc)

## AWS Trusted Advisor
É um serviço que da sugestões pra ti na sua arquitetura AWS, tem tipo um Dashboard que você consegue ver o quanto a sua aplicação "está boa" levando em conta as 5 categorias que ele analisa
- Otimização de Custo
- Performance
- Segurança
- Tolerança a Falha
- Service Limits (latencia?)

---

## AWS Free Tiers
A AWS tem serviços que você pode usar de graça, mas esse "de graça" varia de serviço pra serviço e variam em 3 condições
- Sempre de graça
- 12 meses: Você usa durante 12 meses e depois começa a pagar
- Trial: É uma forma de testar o serviço sem custo, mas isso pode variar de um serviço pra outro, tipo: Horas de uso, tempo de uso, quantidade de chamadas, quantidade de espaço, etc

## AWS Price Calculator
É literalmente uma calculadora pra você estimar quanto vai sair o uso de tais recursos por X tempo. É usado pra se planejar e ter uma ideia dos custos

## Billing Dashboard
É um lugar pra visualizar seus custos, como uso atual, comparar com meses anteriores, etc.

## Consolided Billing
É o lugar que tu consolida as cobranças, basicamente tu tem a visão das contas e organizações como um todo e pode juntar,separar os custos, cobranças, etc. Isso fica dentro da AWS Organizations falado mais acima, lá você já tem a visão de tudo, ai você só aplica o Billing a isso.

## AWS Budgets
É literalmente criar um budget de gasto pra não ultrapssar isso, você pode configurar limites (budgets) por serviço, conta, recurso e até criar alertas pra quando ultrapassar ou estiver perto de ultrapassar.

## AWS Cost Explorer
Similar ao Billing Dashboard mas é um serviço pago e deixa você ir mais a fundo nos detalhes, você pode pesquisar coisas através de queries buscando detalhes como uso por região, por tags, etc.

## AWS Market Place
Digital catalog that includes thousands of software listings from independent software vendors

## AWS Support
Tem tipos diferentes de suport na AWS, sendo eles:
- Basic: Todos tem direito e não custa nada
- Developer: Guia de boas práticas, Ferramentas de Dianostico de erros, suporte de utilização de ferramentas
- Business: Identificar os serviços da AWS, combinações e configurações que podem te ajudar a atender suas necessidades de negócio
  - Tem todos Trusted Advisor Checks: Aquele serviço que analisa sua estrutura em 5 categorias e te dá um feedback em como melhorar, etc
- Enterprise: Mais foda que o Business, também tem o Trusted Advisor e umas paradas a mais ai

---

## AWS Cloud Adoption Framework (AWS CAF)
É um framework criado pela AWS para ajudar com a migração para Cloud, ele define alguns pilares que devem ser seguidos, sendo divididos em

#### Grupos
- Business: Se alinha com o time de TI para que os esforços/investimentos sejam direcionados a conseguir os business values
- People: Suportam e ajudam a mudança de mindset da organização em relação a Cloud
- Governance: Alinham esforços do Business e IT para minimizar riscos riscos e potencializar business value
- Plataform: Definem e seguem os principios e padrões 
- Security: Basicamente cuida da parte de segurança 
- Operations: Ajuda você a habilitar, executar, usar, operar e recuperar cargas de trabalho de TI no nível acordado com as partes interessadas do seu negócio


#### Estratégias
- Rehosting: Só jogar a solução pra Cloud sem mexer em nada
- Replatforming: Fazer alguns ajustes de otimização de cloud e jogar para lá
- Refactoring/re-architecting: Mudar como a solução funciona hoje em dia, basicamente refatorar para usar soluções cloud-native
- Repurchasing: Basicamente sair de um software proprietário para comprar um que resolva o mesmo problema, tipo sair de um customer relationship management (CRM) para o SalesForce, então isso envolve comprar a licença.
- Retaining: Manter as aplicações que são CORE da empresa no ambiente delas (não migrar pra Cloud)
- Retiring: Aposentar solução que não é mais necessária

---

## AWS Snow Family
Basicamente dispositivos fisicos pra tu migrar colocar teus dados e migrar para dentro/fora da AWS, isso pensando que transferir grandes quantias pela internet podem levar meses e até anos, então tem dispositivos fisicos que a AWS entrega pra tu poder colocar teus dados e fazer a transferencia mais rapido através de cabos e forma fisica

- AWS Snowcome: O mais simples de todos, armazena até 14 TB
- AWS Snowball: Intermediário, tem 2 versões, Storage e Compute Optimized como o nome já sugere. O Storage optimized é mais lento mas guarda mais dados (até 80 TB), enquanto o outro até 39 TB +-
- AWS SnowMobile: A porra de um caminhão com container todo seguro e refrigerado que vai até sua porta pra tu carregar com os dados. Essa porra aguenta até 100 PB de Dados. Puta que pariu

---

## Amazon SegeMaker
Pra Machine Learning

## Amazon Code Whisperer
Meio que um sonar, ele tá sugestões de melhoria de código

## Amazon Transcribe
Converte fala em texto

## Amazon Comprehend
Acha padrões em texto

## Amazon Fraud Detector
Identifica possíveis atividades fraudulentas

## Amazon Lex
Pra desenvolver ChatBots

