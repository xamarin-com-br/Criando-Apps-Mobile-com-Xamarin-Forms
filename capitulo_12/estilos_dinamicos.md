## Estilos dinâmicos {#estilos-din-micos}

A classe de estilo não deriva de &quot;BindableObject&quot; e não responde internamente a mudanças em suas propriedades. Por exemplo, se você atribuir um estilo e opor-se a um elemento, e em seguida, modificar um dos objetos &quot;setter&quot;, dando-lhe um novo valor, o novo valor não vai aparecer no elemento. Da mesma forma, o elemento de destino não vai mudar se você adicionar um &quot;setter&quot; ou remover um &quot;setter&quot; da coleção &quot;setters&quot;. Para estes novos &quot;setters&quot; de propriedade terem efeito, você precisa usar o código para destacar o estilo do elemento, definindo a propriedade Estilo para nulo, e em seguida, anexar novamente o estilo para o elemento.

No entanto, sua aplicação pode responder ao estilo mudando dinamicamente em tempo de execução através da utilização de &quot;DynamicResource&quot;. Você deve se lembrar que &quot;DynamicResource&quot; é semelhante ao &quot;StaticResource&quot; aonde ele usa uma chave de dicionário para buscar um objeto ou um valor de um dicionário de recurso. A diferença é que &quot;StaticResource&quot; é um dicionário de pesquisa de uma só vez enquanto &quot;DynamicResource&quot; mantém uma ligação para a real chave de dicionário. Se a entrada do dicionário associada com essa chave é substituída por um novo objeto, essa alteração será propagada para o elemento.

Esta facilidade permite que um aplicativo implemente um recurso chamado às vezes de estilos dinâmicos. Por exemplo, você pode incluir uma instalação em seu programa para temas estilísticos (envolvendo fontes e cores talvez), e você pode fazer estes temas selecionáveis pelo usuário. O aplicativo pode alternar entre estes temas porque eles são implementados com estilos.

Não há nada em um estilo que indique que é um estilo dinâmico. Um estilo torna-se dinamicamente único sendo referenciado usando &quot;DynamicResource&quot; ao invés de StaticResource&quot;.

A seção recursos define quatro estilos: um estilo simples, com a tecla baseButtonStyle&quot;, e em seguida, três estilos que derivam daquele estilo com as teclas &quot;buttonStyle1&quot;, &quot;buttonStyle2&quot; , e &quot;buttonStyle3&quot;.

No entanto, os quatro elementos do botão em direção a parte inferior do arquivo &quot;XAML&quot; usam &quot;DynamicResource&quot; para fazer referência a um estilo com a chave mais simples &quot;ButtonStyle&quot;. Onde está o estilo com essa chave? Isso não existe. No entanto, como as quatro propriedades estilo são definidas com &quot;DynamicResource&quot;, a falta da chave de dicionário não é um problema. Nenhuma exceção é levantada. Mas nenhum estilo é aplicado, o que significa que os botões têm uma aparência padrão:

Cada um dos quatro elementos &quot;Button&quot; tem um manipulador &quot;Click&quot; em anexo, e no arquivo &quot;code-behind&quot;, os primeiros três manipuladores definem uma entrada do dicionário com a tecla &quot;ButtonStyle&quot; para um dos três estilos numerados já definidos no dicionário:

Quando você pressiona um dos três primeiros botões, todos os quatro obtém o estilo selecionado. Aqui está o programa executando em todas as três plataformas que mostram os resultados (da esquerda para a direita) quando os botões 1, 2, e 3 são pressionados:

Pressionando o quarto botão tudo retorna as condições iniciais, definindo o valor associado a a tecla &quot;ButtonStyle&quot; como nulo. (Você também pode considerar chamar &quot;Remove&quot; ou &quot;Clean&quot; no objeto &quot;ResourceDictionary&quot; para remover a chave inteiramente, mas que não funciona na versão &quot;Xamarin.Forms&quot; utilizada neste capítulo.)

Suponha que você queira derivar outro estilo do estilo com a tecla &quot;ButtonStyle&quot;. Como você faz isso no XAML, considerando que a entrada de dicionário &quot;ButtonStyle&quot; não existe até que um dos primeiros três botões é pressionado?

Você não pode fazer desta maneira:

&quot;StaticResource&quot; irá lançar uma exceção se a tecla &quot;ButtonStyle&quot; não existir, e mesmo se a chave existir, o uso de &quot;StaticResource&quot; não vai permitir que mudanças na entrada do dicionário sejam refletidas neste novo estilo.

No entanto, alterar &quot;StaticResource&quot; para &quot;DynamicResource&quot; não vai funcionar:

&quot;DynamicResource&quot; funciona apenas com propriedades garantidas por propriedades vinculáveis ​​, e que não é o caso aqui. Estilo não deriva de &quot;bindableobject&quot;, por isso não pode suportar propriedades vinculáveis.

Em vez disso, estilos definem uma propriedade especificamente para o fim de herdar estilos dinâmicos. A propriedade é a &quot;BaseResourceKey&quot;, que se destina a ser fixada diretamente a uma chave de dicionário que pode ainda não existir ou cujo valor pode mudar dinamicamente, que é o caso com a tecla &quot;ButtonStyle&quot;:

O uso de &quot;BaseResourceKey&quot; é demonstrado pelo projeto &quot;DynamicStylesInheritance&quot;, que é muito semelhante ao projeto &quot;DynamicStyles&quot;. Na verdade, o processamento de código subjacente é idêntico. Na parte inferior da seção de recursos, um novo estilo é definido com uma chave de &quot;newButtonStyle&quot; que usa &quot;BaseResourceKey&quot; para referenciar a entrada &quot;ButtonStyle&quot; e adicionar um par de propriedades, incluindo um que utiliza &quot;OnPlatform&quot;:

Repare que os primeiros três elementos &quot;Button&quot; referenciam a entrada de dicionário &quot;newButtonStyle&quot; com &quot;StaticResource&quot;. &quot;DynamicResource&quot; não é necessária aqui, porque o objeto de estilo associado ao &quot;newButtonStyle&quot; em si não vai mudar, exceto para o estilo que deriva dele. O estilo com a tecla &quot;newButtonStyle&quot; mantém uma ligação com &quot;ButtonStyle&quot; e, internamente, altera-se quando ocorrem mudanças de estilo. Quando o programa começa a ser executado, apenas as propriedades definidas na &quot;newButtonStyle&quot; são aplicadas a esses três botões:

O botão &quot;Reset&quot; continua a fazer referência a entrada &quot;ButtonStyle&quot;.

Como no programa &quot;DynamicStyles&quot;, o arquivo &quot;code-behind&quot; define a entrada de dicionário quando você clica em um dos três primeiros botões, para que todos os botões peguem as propriedades &quot;ButtonStyle&quot; também. Aqui estão os resultados para (da esquerda para a direita) os cliques dos botões 3 , 2 ​​e 1: