## Visualizações Separadas por Identificadores {#visualiza-es-separadas-por-identificadores}

No programa **TwoButtons** você observa uma técnica de compartilhamento de gerenciamento de eventos que separa as visualizações comparando os objetos. Isto funciona bem quando não existem muitas visualizações, mas não seria muito adequado para um programa de calculadora.

A classe Element define uma propriedade StyleId do tipo string com o objetivo específico de identificar as visualizações. Isto pode ser utilizado onde quer que seja conveniente na aplicação. Você pode testar os valores utilizando declarações de bloco como if/else, switch/case ou você pode utilizar um método Parse para converter strings em números ou enumerações.

O programa a seguir não é uma calculadora, mas sim um teclado numérico, o que certamente pode ser parte de uma calculadora. Este programa é chamado **SimplestKeypad** e utiliza um StackLayout para organizar as linhas e colunas das teclas. (Um dos objetivos do programa é demonstrar que StackLayout não necessariamente seria a ferramenta correta para este trabalho!)

O programa cria um total de cinco instâncias de StackLayout. The mainStack tem orientação vertical com quatro objetos StackLayout horizontais dispostos em 10 botões para os dígitos. Para manter a simplicidade, o teclado numérico está ordenado como um teclado de telefone ao invés de teclado de calculadora.

Todas as 10 teclas numéricas compartilham um único gerenciador Clicked. A propriedade StyleId indica o número que está associado a tecla então o programa pode simplesmente adicionar o número na string mostrada no Label. O StyleId é idêntico a propriedade Text do Button então Text poderia ser utilizada ao invés de StyleId, mas em geral, isto não seria assim tão conveniente.

O botão voltar funciona de forma diferente tendo o seu próprio gerenciador Clicked mesmo sabendo que seria possível combinar os dois métodos em apenas um para obter a vantagem de escrever apenas um código para endereçar o que eles têm em comum. Para deixar o teclado com um tamanho um pouco maior, o tamanho da fonte de texto FontSize é ajustado utilizando NamedSize.Large. Aqui estão as três telas renderizadas pelo programa **SimplestKeypad**:

É claro que você poderia pressionar as teclas repetidamente para ver como o programa responde a uma grande quantidade de digitos e descobrir que ele não está preparado para isto. Quando o conteúdo do Label se torna muito grande ele passa a comandar o comprimento total do StackLayout vertical e os botões também começam a se movimentar.

Mesmo antes disso, você poderia notar algumas pequenas irregularidades no comprimento do Button, particularmente no Windows Phone. Os comprimentos individuais dos objetos Button são baseados no seu conteúdo, e para dígitos decimais, muitas fontes tem comprimento diferentes. Seria possível resolver este problema usando Expands flag na propriedade HorizontalOptions? Não. A flag Expands acaba adicionando espaços extras igualmente distribuidos pelas visualizações no StackLayout. Cada visualização irá aumentar linearmente a mesma quantidade então mesmo assim elas não estarão com o mesmo comprimento. Por exemplo, de uma olhada em dois botões no programa **TwoButtons** ou no **ButtonLambdas**. Eles têm as suas propriedades HorizontalOptions inicializadas como FillAndExpand, mas elas têm comprimentos diferentes porque os comprimentos do conteúdo do Button são diferentes.

Uma solução melhor para estes programas seria utilizar outro recurso conhecido como Grid que será abordado no capítudo 18.