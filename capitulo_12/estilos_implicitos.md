## Estilos Implícitos {#estilos-impl-citos}

Cada entrada em um &quot;ResourceDictionary&quot; requer uma chave de dicionário. Este é um fato indiscutível. Se você tentar passar uma chave nula para o método &quot;Add&quot; de um objeto &quot;ResourceDictionary&quot;, você vai disparar uma exceção &quot;Argument-NullException&quot;.

No entanto, há um caso especial aonde um programador não é necessário para fornecer essa chave dicionário e essa chave é em vez disso gerada automaticamente.

Este caso especial é para um objeto &quot;Style&quot; adicionado a um &quot;ResourceDictionary&quot; sem uma configuração &quot;x: Key&quot;. O &quot;ResourceDictionary&quot; gera uma chave com base no &quot;Targettype&quot;, que é sempre necessária. (Uma breve exploração revelará que esta chave dicionário especial é o nome completo associado com o &quot;TargetType&quot; do estilo. Para um TargetType Button, por exemplo, a chave de dicionário é &quot;Xamarin.Forms.Button&quot;. Mas você não precisa saber disso.)

Você também pode adicionar um estilo a um &quot;ResourceDictionary&quot; sem uma chave de dicionário em código: uma sobrecarga do método &quot;Add&quot; aceita um argumento do tipo estilo, mas não exige qualquer outra coisa.

Um objeto de estilo em um &quot;ResourceDictionary&quot; que tem uma dessas chaves geradas é conhecido como um estilo implícito, e a chave de dicionário gerada é muito especial. Você não pode se referir a essa chave usando diretamente &quot;StaticResource&quot;. No entanto, se um elemento dentro do âmbito da &quot;ResourceDictionary&quot; tem o mesmo tipo que a chave do dicionário, e se esse elemento não tiver sua propriedade de estilo explicitamente definida para outro objeto &quot;Style,&quot; então este estilo implícito é aplicado automaticamente.

O seguinte &quot;XAML&quot; do projeto &quot;ImplicitStyle&quot; demonstra isso. É o mesmo que o arquivo &quot;XAML&quot; &quot;BasicStyle&quot;, exceto que o estilo não tem configuração &quot;x : Key&quot; e as propriedades de estilo nos botões não são definidas usando &quot;StaticResource&quot;:

Apesar da ausência de qualquer ligação explícita entre os botões e o estilo, o estilo é definitivamente aplicado:

Um estilo implícito é aplicado somente quando a classe do elemento coincide com o &quot;TargetType&quot; do estilo exatamente. Se você incluir um elemento que deriva de &quot;Button&quot; no &quot;StackLayout&quot;, não terá o estilo aplicado.

Você pode usar as configurações de propriedade locais para substituir propriedades definidas através do estilo implícito, assim como você pode substituir configurações de propriedade em um estilo definido com &quot;StaticResource&quot;.

Você vai achar estilos implícitos um recurso muito poderoso e extremamente útil. Sempre que você tem várias &quot;Views&quot; do mesmo tipo e você determinar que você quer que todas elas tenham uma configuração de propriedade idêntica ou dois, é muito fácil de definir rapidamente um estilo implícito. Você não tem que tocar os próprios elementos.

No entanto, com tanto poder pelo menos alguma responsabilidade o programador tem. Porque nenhum estilo é referência nos próprios elementos, o que pode ser confuso quando examinado o &quot;XAML&quot; para determinar se alguns elementos são estilizados ou não. Por vezes, a aparência de uma página indica que um estilo implícito é aplicado a alguns elementos, mas não é bastante óbvio onde o estilo implícito é definido. Se você então quiser mudar esse estilo implícito, você tem que procurar manualmente por ele na árvore visual.

Por esta razão, você deve definir estilos implícitos tão perto quanto possível dos elementos aos quais eles são aplicados. Se as &quot;Views&quot; que estão utilizando o estilo implícito estão em um determinado &quot;StackLayout&quot;, em seguida, definia o estilo implícito na seção Recursos neste &quot;StackLayout&quot;. Um comentário ou dois poderiam ajudar a evitar a confusão também.

Curiosamente, estilos implícitos têm uma restrição interna que pode persuadi-lo a mantê-los perto para os elementos aos quais são aplicados. Aqui está a restrição: Você pode derivar um estilo implícito de um estilo com uma chave de dicionário explícito, mas você não pode fazer o caminho inverso. Você não pode usar &quot;BasedOn&quot; para fazer referência a um estilo implícito.

Se você definir uma cadeia de estilos que utilizam &quot;BasedOn&quot; para derivar um do outro, o estilo implícito ( se houver) estará sempre no final da cadeia. Não será possível a existência de outras derivações.

Isto implica que você pode estruturar seus estilos com três tipos de hierarquias:

*   **A partir de estilos definidos sobre a Aplicação e &quot;Page Down&quot; para estilos definidos em &quot;layouts&quot; mais abaixo na árvore visual.**
*   **A partir de estilos definidos para classes de base, tais como &quot;VisualElement&quot; e &quot;View&quot; para estilos definidos para classes específicas.**
*   **A partir de estilos com chaves de dicionário explícitas para estilos implícitos.**

Isto é demonstrado no projeto &quot;StyleHierarchy&quot;, que utiliza um semelhante (mas de certa forma simplificado) conjunto de estilos como você viu no início do projeto &quot;StyleInheritance&quot;. No entanto, esses estilos estão agora espalhados em mais de três seções de recursos.

Usando uma técnica que você viu no programa &quot;ResourceTrees&quot; no capítulo 10 , o projeto &quot;StyleHierarchy&quot; recebeu uma classe &quot;App&quot; baseada em &quot;XAML&quot;. A classe &quot;App.xaml&quot; tem um &quot;ResourceDictionary&quot; contendo um estilo com apenas uma propriedade &quot;Setter&quot;:

Em um aplicativo de várias páginas, este estilo seria usado em todo o aplicativo.

O arquivo &quot;code-behind&quot; da classe &quot;App&quot; chama o método InitializeComponent para processar o arquivo &quot;XAML&quot; e definir a propriedade &quot;MainPage&quot;:

O arquivo &quot;XAML&quot; da classe de página define um estilo para a página inteira que deriva do estilo na classe &quot;App&quot; e, em seguida, dois estilos implícitos que derivam do estilo para a página. Observe que a propriedade de estilo da página está definida para o estilo definido na classe &quot;App&quot;:

Os estilos implícitos são definidos como próximos aos elementos de destino quando possível.

Aqui está o resultado:

O incentivo para separar objetos de estilo em dicionários separados não faz muito sentido para pequenos programas como este, mas para programas maiores, torna-se tão importante ter uma hierarquia estruturada de definições de estilos quanto ter uma hierarquia estruturada de definições de classe.

As vezes você vai ter um estilo com uma chave de dicionário explícita (por exemplo, &quot;myButtonStyle&quot;), mas você vai querer que o mesmo estilo seja implícito também. Basta definir um estilo com base nessa chave com nenhuma tecla ou &quot;setters&quot; próprios: