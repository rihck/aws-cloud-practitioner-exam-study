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


