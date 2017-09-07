## Gerenciadores de Eventos Anônimos {#gerenciadores-de-eventos-an-nimos}

Nos dias de hoje muitos programadores C# tendem a definir gerenciadores de eventos anônimos como funções lambda. Isto permite que o código dos gerenciadores de eventos fiquem próximos do instanciamento e inicialização dos objetos disparando eventos ao invés de estarem em qualquer outro lugar no arquivo. Isto também permite que os objetos sejam referenciados de dentro do gerenciador de eventos sem precisar armazenar estes objetos como campos.

Observe abaixo o programa chamado **ButtonLambdas** que contém um Label para mostrar um número e mais dois botões. Um dos botões dobra o valor do número e o outro divide. Normalmente, as variáveis number e Label seriam salvas como campos. Mas, devido a utilização de gerenciamento de eventos anônimo diretamente no construtor depois que elas já estão definidas, os gereciadores de eventos têm acesso as mesmas:

Note o uso de Device.GetNamedSize para obter o tamanho da fonte de texto para os objetos Label e Button. O segundo argumento de GetNamedSize é diferente para cada um dos tipos de objeto obtendo assim o tamanho apropriado para cada um deles. Como demonstrado no programa anterior, os dois botões ficam dispostos horizontalmente no mesmo StackLayout:

A desvantagem na utilização de gerenciadores de eventos como funções lambda é que eles não podem ser re-utilizados em diferentes visualizações. (Na verdade, seria possível, mas para isto seria necessário utilizar técnicas de codificação baseadas em reflection que acabariam deixando o código bem mais complexo)