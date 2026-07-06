# Visão geral
- Atividades e o que são
- Limitações
- Campos criados por padrão
- Quando usar uma activity
- QUando não usar uma activity


## Atividades e o que são
Atividades são um tipo de tabela do dynamics, que possui alguns comportamentos padrão, como fazer parte do grupo de "atividades", e filha de "*activitypointer*", que é a tabela que armazena todas as atividades.

Exemplos de atividades nativas: Email, Phone Call, Task, Appointment, Letter, Fax E Social Activity

## Limitações
Uma activity por padrão, só aparece na timeline de 1 registro, que é definido como seu "registro pai", o *regardingobjectid*, não possuindo suporte para exibir a mesma activity em várias timeliens nativamente

(EXCETO por Email, que possui os campos *to*, *from*, *cc*, *bcc*, e aparecerá na timeline dos registros relacionados)

Limite de 10 card forms na timeline. Se você habilitar mais de 10 tipos de activity com card form customizado, o controle para de funcionar e exibe erro. O fetchXML gerado internamente tem limite de 10 link-entities.

Entidades do tipo Activity não podem ter timeline própria (nesse caso, não podem exibir OUTRAS activities em sua timeline) - Pode se resolver criando uma subgrid 1:N

Sem refresh automático (Necessário atualizar para a timeline atualizar)

## Campos criados por padrão
Ao criar uma tabela como tipo Activity, ela recebe automaticamente
- activityid (PK) 
- regardingobjectid 
- regardingobjecttypecode 
- subject 
- scheduledstart 
- scheduledend 
- actualstart 
- actualend 
- statecode
- statuscode
- ownerid
- prioritycode

entre outros. Você não precisa criar esses campos manualmente.

## Quando usar activity
Quando a entidade(tabela) será utilizada para a comunicação, precisando aparecer na timeline de outras entidades

Quando se faz interação com activity monitor ou rastreamento de processos automáticos

## Quando NÃO USAR activity

- Quando você precisa de uma timeline própria exibindo OUTRAS atividades e tabelas
- precisa que o registro apareça em múltiplas timelines simultaneamente (não suportado para custom activities)
- Quando a entidade é mais um "registro de dados", ou seja, não é algo para interagir com o cliente