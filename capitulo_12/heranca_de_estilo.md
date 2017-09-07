## Herança de estilo {#heran-a-de-estilo}

O &quot;TargetType&quot; do estilo faz duas funções diferentes: Uma dessas funções é descrita em a próxima seção sobre estilos implícitos. A outra função é para o benefício do analisador &quot;XAML&quot;. O analisador &quot;XAML&quot; deve ser capaz de resolver os nomes de propriedades nos objetos &quot;Setter&quot; e para isso ele precisa que seja fornecido o nome de uma classe pelo &quot;TargetType&quot;.

Todas as propriedades no estilo devem ser definidas por ou herdadas pela classe especificada na propriedade &quot;TargetType&quot;. O tipo do elemento visual no qual o estilo é definido deve ser o mesmo &quot;TargetType&quot; ou uma classe derivada de &quot;TargetType&quot;.

Se você precisar de um estilo apenas para propriedades definidas por &quot;View&quot;, você pode definir o &quot;TargetType&quot; para &quot;View&quot; e ainda usar o estilo nos botões ou qualquer outro derivado de &quot;View&quot;, como nesta versão modificada do programa &quot;BasicStyle&quot;:

Como você pode ver, o mesmo estilo é aplicado a todos os &quot;Button&quot; e &quot;Label&quot; filhos do &quot;StackLayout&quot; :

Mas supomos que agora você quer expandir este estilo, mas de forma diferente para &quot;Button&quot; e &quot;Label&quot;. Isso é possível?

Sim. Os estilos podem derivar de outros estilos. A classe estilo inclui uma propriedade chamada &quot;BasedOn&quot; do tipo estilo. No código, você pode definir essa propriedade &quot;BasedOn&quot; diretamente a outro objeto estilo. Em &quot;XAML&quot; você definir o atributo &quot;BasedOn&quot; para uma extensão de marcação &quot;StaticResource&quot; que faz referência a um estilo criado anteriormente. O novo estilo pode incluir objetos &quot;Setter&quot; para novas propriedades ou usá-las para substituir propriedades no estilo anterior. O estilo &quot;BasedOn&quot; deve ter como alvo a mesma classe ou uma classe ancestral do novo estilo &quot;TargetType&quot;.

Aqui está o arquivo XAML para um projeto chamado &quot;StyleInheritance&quot;. O aplicativo faz uma referência para&quot;Xamarin.FormsBook.Toolkit&quot; para duas finalidades: ele usa a extensão de marcação &quot;HslColor&quot; para demonstrar que as extensões de marcação são ajustes dos valores legítimos em objetos &quot;Setter&quot; e para demonstrar que um estilo pode ser definido para uma classe customizada, neste caso &quot;AltLabel&quot;.

O &quot;ResourceDictionary&quot; contém quatro estilos : O primeiro tem uma chave de dicionário de &quot;VisualStyle&quot;. O estilo com a chave do dicionário de &quot;baseStyle&quot; deriva de &quot; VisualStyle&quot;. Os estilos com teclas de &quot;labelStyle &quot;e&quot; ButtonStyle&quot; derivam de &quot;baseStyle &quot;. O Estilo do Botão demonstra como definir uma propriedade de valor de um &quot;Setter&quot; a um objeto &quot;OnPlatform&quot;:

Imediatamente após a seção de recursos uma marcação define a propriedade estilo da página com o estilo &quot;VisualStyle&quot;:

Como a página deriva de &quot;VisualElement&quot; mas de &quot;View&quot;, este é o único estilo no recurso dicionário que pode ser aplicada para a página. No entanto, o modelo não pode ser aplicado a página até depois da seção de recursos, portanto, usar o elemento &quot;form&quot; de &quot;StaticResource&quot; é uma boa solução aqui. O fundo inteiro da página é colorido com base neste estilo, e o estilo também é herdado por todos os outros estilos:

Se o estilo para o &quot;AltLabel&quot; só incluiu objetos &quot;setter&quot; para propriedades definidas por &quot;Label&quot;, o &quot;Targettype&quot; poderia ser &quot;Label&quot; em vez de &quot;AltLabel&quot;. Mas o estilo tem um &quot;Setter&quot; para a propriedade &quot;PointSize&quot;. Essa propriedade é definida por &quot;AltLabel&quot; de modo que o &quot;TargetType&quot; deve ser &quot;tookit:AltLabel&quot;.

Um &quot;setter&quot; pode ser definido para a propriedade &quot;PointSize&quot; porque &quot;PointSize&quot; é apoiado por uma propriedade &quot;bindable&quot;. Se você muda a acessibilidade do objeto &quot;BindableProperty&quot; em &quot;AltLabel&quot; de público para privado, a propriedade continuará a funcionar para muitos usos rotineiros de &quot;AltLabel&quot;, mas agora &quot;PointSize&quot; pode não ser um &quot;Setter&quot; de estilo. O analisador XAML se queixa de que ele não pode encontrar &quot;PointSizeProperty&quot;, que é a propriedade &quot;bindable&quot; que faz a propriedade &quot;PointSize&quot;.

Você descobriu no capítulo 10 como &quot;StaticResource&quot; funciona: Quando o analisador &quot;XAML&quot; encontra uma extensão de marcação &quot;StaticResource&quot; ele busca uma árvore visual para uma chave de dicionário. Este processo tem implicações para estilos. Você pode definir um estilo em uma seção de recursos e em seguida, sobreescrever capítulo 12 estilos 252 com outro estilo com a mesma chave de dicionário em uma seção de recursos diferente localizada mais abaixo na árvore visual. Quando você definir a propriedade &quot;BasedOn&quot; para uma extensão de marcação &quot;StaticResource&quot;, o estilo que você está derivando deve ser definido na mesma seção Recursos (como demonstrado no programa &quot;StyleInheritance&quot; ) ou uma seção de recursos superior na árvore visual.

Isto significa que você pode estruturar seus estilos em &quot;XAML&quot; de duas maneiras hierárquicas: você pode usar &quot;BasedOn&quot; para derivar estilos de outros estilos, e você pode definir estilos em diferentes níveis na árvore visual que derivam de estilos mais elevados na árvore visual ou substituí-los por completo.

Para aplicações maiores, com várias páginas e muita marcação, a recomendação para a definição de estilos é muito simples: definir seus estilos tão perto quanto possível dos elementos que usam esses estilos.

Aderir a estas recomendações para manter o programa é particularmente importante quando se trabalha com estilos implícitos.