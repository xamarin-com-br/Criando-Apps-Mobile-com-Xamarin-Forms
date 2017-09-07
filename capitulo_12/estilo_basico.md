## Estilo básico {#estilo-b-sico}

No capítulo 10, &quot;extensões de marcação XAML&quot;, você viu um trio de botões que continha uma grande quantidade de marcação idêntica. Aqui estão eles novamente:

Com exceção da propriedade de texto, todos os três botões têm as mesmas configurações de propriedade. Uma solução parcial para essa marcação repetitiva envolve a definição de valores de propriedade de um recurso dicionário e referenciá-los com a extensão de marcação &quot;StaticResource&quot;. Como você viu no projeto &quot;ResourceSharing&quot; no Capítulo 10, esta técnica não reduz o volume de marcação, mas ela faz consolidar os valores em um só lugar. Para reduzir o volume de marcação, você vai precisar de um estilo. Um objeto de estilo é quase sempre definido em um &quot;ResourceDictionary&quot;. Geralmente, você vai começar com uma seção de recursos na parte superior da página:

Instancie o estilo com tags de inicio e fim separados

Porque o estilo é um objeto em um &quot;ResourceDictionary&quot;, você precisará de um atributo &quot;x: Key&quot; para dar-lhe uma chave de dicionário descritiva. Você também deve definir a propriedade &quot;TargetType&quot;. Este é o tipo de elemento visual para qual o estilo é projetado, que neste caso é &quot;Button&quot;.

Como você verá na próxima seção deste capítulo, você também pode definir um estilo no código. No código, o construtor do estilo requer um objeto do tipo &quot;Type&quot; para a propriedade &quot;TargetType&quot;. A propriedade &quot;TargetType&quot; não tem um método &quot;set&quot; público para acesso; portanto, a propriedade &quot;Targettype&quot; não pode ser alterada após o estilo ser criado.

&quot;Style&quot; também define outra propriedade disponível somente por método &quot;get&quot; importante chamada &quot;Setters&quot; do tipo IList &lt; Setter&gt;, que é uma coleção de objetos &quot;Setter&quot;. Cada Setter é responsável por definir a configuração da propriedade no estilo. A classe &quot;Setter&quot; define apenas duas propriedades:

*   &quot;Property&quot; do tipo &quot;BindableProperty&quot;.
*   &quot;Value&quot; of type &quot;Object&quot;.

As propriedades definidas no estilo devem ser apoiadas por propriedades vinculáveis​​! Mas quando você definir a propriedade em XAML, não use a propriedade &quot;name&quot;. Apenas especifique o texto, que é o mesmo que o nome da propriedade CLR relacionada. Aqui está um exemplo:

O &quot;XAML parser&quot; usa as classes TypeConverter familiares ao analisar as configurações de valor destas instâncias &quot;Setter&quot;, para que você possa usar as mesmas configurações de propriedade que você usa normalmente. &quot;Setters&quot; é a propriedade de conteúdo de estilo, assim você não precisa das tags Style.Setters para adicionar objetos &quot;Setter&quot; ao &quot;Style&quot;.

O passo final é definir esse objeto de estilo para a propriedade estilo de cada botão. Use a extensão familiar de marcação &quot;StaticResource&quot; para referenciar a chave dicionário. Aqui está o arquivo XAML completo no projeto &quot;BasicStyle&quot;:

Agora todas essas configurações de propriedade estão em um objeto &quot;Style&quot; que é compartilhado entre múltiplos elementos &quot;Button&quot;.

Os efeitos visuais são os mesmos que aqueles do programa &quot;ResourceSharing&quot; no capítulo 10, mas a marcação é muito mais concisa. Supomos que você quisesse definir um &quot;Setter&quot; para o &quot;TextColor&quot; usando o método estático Color.FromHsla. Você pode definir uma cor usando o atributo &quot;x: FactoryMethod&quot;, mas como você pode possivelmente definir um pedaço tão pesado de marcação para a propriedade &quot;Value&quot; do objeto Setter? Você pode usar a sintaxe &quot;property-element&quot;, é claro!

Sim, você pode expressar a propriedade &quot;Value&quot; do &quot;Setter&quot; com &quot;property-element tags&quot; e, em seguida, definir o conteúdo para qualquer objeto do tipo &quot;Color&quot;. Aqui está outra maneira de fazê-lo: Definir o valor da cor como um item separado no dicionário de recursos e então usar &quot;StaticResource&quot; para configurá-lo para a propriedade &quot;Value&quot; do &quot;Setter&quot;:

Esta é uma boa técnica se você está compartilhando a mesma cor entre vários estilos ou múltiplos &quot;Setters&quot;. Você pode sobrescrever a propriedade &quot;setting&quot; de um estilo definindo com uma propriedade diretamente no elemento visual. Note que o segundo botão tem sua propriedade TextColor definida como marrom:

O botão central terá texto marrom, enquanto os outros dois botões terão suas configurações &quot;TextColor&quot; vindas do estilo. Uma propriedade definida diretamente no elemento visual é chamado às vezes de configuração local ou configuração manual, e ela sempre sobrescreve a propriedade configurada pelo estilo. O objeto &quot;Style&quot; no programa &quot;BasicStyle&quot; é compartilhado entre os três botões. O compartilhamento de estilos tem uma aplicação importante para os objetos &quot;Setter&quot;. Qualquer objeto definido para a propriedade &quot;Value&quot; de um &quot;Setter&quot; deve ser compartilhável. Não tente fazer algo parecido com isto:

Este &quot;XAML&quot; não funciona por duas razões: Conteúdo não é apoiado por uma &quot;BindableProperty&quot; e portanto não pode ser utilizado em um &quot;Setter&quot;. Mas a intenção óbvia aqui é para cada &quot;Frame&quot;, ou pelo menos para cada estrutura em que este estilo é aplicado, obter esse mesmo objeto &quot;Label&quot; como conteúdo. Um objeto &quot;Label&quot; único não pode aparecer em vários lugares na página. Uma maneira muito melhor de fazer algo parecido com isto é derivar uma classe de &quot;Frame&quot; e definir uma &quot;Label&quot; como a propriedade de conteúdo, ou derivar uma classe de &quot;ContentView&quot; que inclui um &quot;Frame&quot; e um &quot;Label&quot;.

Você pode querer usar um estilo para definir um manipulador de eventos para um evento como um &quot;click&quot;. Isso seria útil e conveniente, mas não é suportado. Manipuladores de eventos deve ser definidos nos próprios elementos. (No entanto, a classe &quot;Style&quot; faz objetos de apoio chamados gatilhos, que podem responder a eventos ou mudanças de propriedade. Gatilhos serão discutidos em um capítulo futuro.)

Você não pode definir a propriedade &quot;GestureRecognizers&quot; em um estilo. Isso seria útil, mas &quot;GestureRecognizers&quot; não é apoiada por uma propriedade de ligação.

Se uma propriedade de ligação é um tipo de referência, e se o valor padrão é nulo, você pode usar um estilo para definir a propriedade para um objeto não nulo. Mas você pode também querer sobrescrever esta configuração de estilo com uma configuração local que defina a propriedade de volta para nulo. Você pode definir uma propriedade como nula em &quot;XAML&quot; com a extensão de marcação &quot;{ x: Null }&quot;.