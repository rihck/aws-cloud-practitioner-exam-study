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

## 