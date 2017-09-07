## Extensões de marcação Menos Divulgadas {#extens-es-de-marca-o-menos-divulgadas}

Três extensões marcação não são usadas tanto quanto outras. Estas são:

*   **x: nulo**
*   **x: Type**
*   **x: Array**

Você usa a extensão x:Null para definir uma propriedade para nulo. A sintaxe parecida com esta:

Isso não faz muito sentido, a menos que SomeProperty tem um valor padrão que não é nulo quando é desejavel para definir a propriedade para nulo . Mas, como você verá no Capítulo 12, por vezes, uma propriedade pode adquirir um valor não-nulo de um estilo, e x:null é praticamente a única maneira de substituir isso.

A extensão de marcação x:Type é usada para definir uma propriedade do tipo Type , a classe .NET descrevendo o tipo de uma classe ou estrutura. Aqui está a sintaxe:

Você também vai usar x: Type em conexão com x: Array. A extensão de marcação x: Array é sempre usada com elementos regulares de sintaxe, em vez de sintaxe chave. Tem um argumento necessário nomeado Type que você configura com a extensão de marcação x:Type. Isto indica o tipo dos elementos na matriz.

Aqui é como uma matriz pode ser definida em um dicionário de recursos: