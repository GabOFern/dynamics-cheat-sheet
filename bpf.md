# Visão Geral
Topicos Gerais
- BPFs (Business Process Flow) o que é
- Tipos de etapas
- Scripts Úteis

## BPFs (Business Process Flow) o que é
São fluxos de trabalho feito em etapas (representadas por nodes) na interface de um formulário.

Cada entidade (tabela) pode ter até 10 BPFs com NO MÁXIMO 30 etapas cada.
Portanto, analise bem se o fluxo que será seguido estará realmente de acordo com essas regras.

(Caso queira aumentar essa limitação é possível, mas será necessário alterar a configuração no XML do ambiente, qualquer erro pode corromper o ambiente inteiro, logo, não recomendado.)

Caso necessário, fica na categoria de "Processos"

## Tipos de etapas
As etapas, são os "nós" que aparecem na interface, tendo os seguintes possiveis status

- Atual: Circulo azul com branco
- Pendente: Circulo cinza/branco
- Finalizado: Todos os circulos azuis, o ultimo azul com uma bandeira

## Iniciando em uma tabela, finalizando em outra
Uma função interessante para os BPFs, é o fato de que você pode transitar entre mais de uma tabela em meio aos estagios
Para isso, será necessário referenciar campos de outra tabela na etapa, possivel ao clicar em "Relacionados"

Na prática, assim que o usuário clicar na etapa onde essa transição ocorre, a tela irá atualizar, indo para essa nova atividade ou tabela (o que também poderá atualizar os formulários)

## Scripts Úteis
### Como capturar se um BPF foi finalizado ou não
Xrm.Page.data.process.getStatus();

### Como capturar o guid do estágio atual do processo
Xrm.Page.data.process.getActiveStage();

### Capturar apenas o BPF que está ativo AGORA no registro
setTimeout(function() {
    try {
        var activeProcess = Xrm.Page.data.process.getActiveProcess();

        if (activeProcess) {
            var processName = activeProcess.getName();
            var processId = activeProcess.getId();
            console.log("--- Processo BPF Ativo ---");
            console.log("Nome do Processo:", processName);
            console.log("GUID do Processo:", processId);
            console.log("--------------------------");
        } else {
            console.log("Nenhum Business Process Flow (BPF) ativo encontrado na página.");
        }
    } catch (error) {
        console.error("Erro ao obter o processo BPF ativo:", error.message);
    }
}, 100); // Pequeno atraso de 100ms para auxiliar na exibição do console

### Capturar TODOS os estágios do bpf atual
(function() {
    // Tentativa de log inicial para verificar se o console está funcionando minimamente
    console.log("--- Iniciando verificação do BPF ---");

    try {
        var activeProcess = Xrm.Page.data.process.getActiveProcess();

        if (activeProcess) {
            console.log("BPF Ativo encontrado!");
            try {
                console.log("Nome do BPF: " + activeProcess.getName());
                var stages = activeProcess.getStages();
                if (stages && stages.getLength() > 0) {
                    console.log("--- Todos os Estágios do BPF Atual ---");
                    stages.forEach(function(stage, index) {
                        console.log("  Estágio " + (index + 1) + ": " + stage.getName() + " -> GUID: " + stage.getId());
                    });
                    console.log("-------------------------------------");
                } else {
                    console.log("Nenhum estágio encontrado para o BPF ativo.");
                }
            } catch (e) {
                console.error("Erro ao processar BPF/Estágios:", e.message);
            }
        } else {
            console.log("Nenhum Business Process Flow (BPF) ativo encontrado na página.");
        }
    } catch (globalError) {
        console.error("Erro inesperado na execução do script:", globalError.message);
    }

    console.log("--- Verificação do BPF Finalizada ---");
})();