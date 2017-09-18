## Estilo básico

No capítulo 10, "extensões de marcação XAML", você viu um trio de botões que continha uma grande quantidade de  marcação idêntica. Aqui estão eles novamente:

![](/assets/12-1-estilo.png)

![](/assets/12-2-estilo.png)

![](/assets/12-3-estilo.png)

Com exceção da propriedade **Text**, todos os três botões têm as mesmas configurações de propriedades.

Uma solução parcial para essa marcação repetitiva envolve a definição de valores de propriedade de um recurso dicionário e referenciá-los com a extensão de marcação "**StaticResource**". Como você viu no projeto "**ResourceSharing**"  no Capítulo 10, esta técnica não reduz o volume de marcação, mas ela consolida os valores em um só lugar. Para reduzir o volume de marcação, você vai precisar de um estilo. Um objeto de estilo é quase sempre definido em um "**ResourceDictionary**". Geralmente, você vai começar com uma seção de recursos na parte superior da página:

![](/assets/12-04REsourceDictionary.png)![](/assets/12-05-resorcedictionay1.png)Instancie o estilo com tags de inicio e fim separados.

![](/assets/12-06-estilo-tags.png)Porque o **estilo **é um objeto em um "**ResourceDictionary**", você precisará de um atributo "**x: Key**" para dar-lhe uma chave de dicionário descritiva. Você também deve definir a propriedade "**TargetType**". Este é o tipo de elemento visual para qual o estilo é projetado, que neste caso é "**Button**".

Como você verá na próxima seção deste capítulo, você também pode definir um estilo no código. No código, o construtor do estilo requer um objeto do tipo "**Type**" para a propriedade "**TargetType**". A propriedade "**TargetType**" não tem um método "**set**" público para acesso; portanto, a propriedade "**Targettype**" não pode ser alterada após o estilo ser criado.

O "**Style**" também define outra propriedade disponível somente por método "**get**" importante chamada "**Setters**" do tipo **IList&lt;Setter&gt;,** que é uma coleção de objetos "**Setter**". Cada **Setter **é responsável por definir a configuração da propriedade no estilo. A classe "**Setter**" define apenas duas propriedades:

* "**Property**" do tipo "**BindableProperty**".
* "**Value**" do tipo  "**Object**".

As propriedades definidas no estilo devem ser suportadas por propriedades vinculáveis​​ \(**bindable**\) ! Mas quando você definir a propriedade em XAML, não use a propriedade "**name**". Apenas especifique o texto, que é o mesmo que o nome da propriedade CLR relacionada. Aqui está um exemplo:

![](/assets/12-07-PropiedadeCLR.png)

O parse "**XAML**" usa as classes **TypeConverter** ao analisar as configurações do valor destas instâncias "**Setter**", para que você possa usar as mesmas configurações da propriedade que você usa normalmente.

"**Setters**" é o conteuúdo da **Style**, assim você não precisa das tags **Style.Setters** para adicionar objetos "**Setter**" ao "**Style**":![](/assets/12-08-SetterStyle.png)![](/assets/12-09-SetterStyler1.png)Mais dois objetos **Setter **são necessários para **BackgroundColor **e **BorderColor**. Estes envolvem o **OnPlatform** e podem, ao princípio, parecer impossíveis de expressar na marcação. No entanto, é possível expressar a propriedade **Value** de **Setter **como um elemento da propriedade, com a marcação **OnPlatform **entre as tags de elemento de propriedade:

![](/assets/12-10-SetterProperty.png)

O passo final é definir esse objeto de **estilo **para a propriedade **Style **de cada botão. Use a extensão familiar de marcação "**StaticResource**" para referenciar a chave no dicionário. Aqui está o arquivo XAML completo no projeto "**BasicStyle**":

![](/assets/12-11-BasciStyle.png)![](/assets/12-12-BasciStyle.png)Agora todas essas configurações de propriedade estão em um objeto "**Style**" que é compartilhado entre múltiplos elementos "Button".

![](/assets/12-12-ExemploTelasStyle.png)

Os efeitos visuais são os mesmos que aqueles do programa "**ResourceSharing**" no capítulo 10, mas a marcação é muito mais concisa.

Suponha que você gostaria de definir um **Setter **para o TextColor usando o método estátic**o Color.FromHsla**. Você pode definir essa cor usando o attributo **x: FactoryMethod**, mas como você pode definir um pedaço de marcação tão difícil na propriedade **Value **do objeto **Setter**? Como você viu anteriormente, a solução é quase sempre sintaxe de elemento de propriedade:

![](/assets/12-13-ValeuSttert.png)

Aqui está outra maneira de fazê-lo: Defina o valor de **Cor **como um item separado no dicionário de recursos e, em seguida, use **StaticResource **para configurá-lo para a propriedade **Value** do **Setter**:

![](/assets/12-14-OutraManeira.png)

Esta é uma boa técnica se você está compartilhando a mesma **cor **entre vários estilos ou múltiplos "**Setters**".

Você pode sobrescrever a propriedade "**Style**" de um estilo definindo com uma propriedade diretamente no elemento visual. Note que o segundo botão tem sua propriedade **TextColor **definida como marrom:

![](/assets/12-14-TextColor.png)

O botão central terá texto marrom, enquanto os outros dois botões terão suas configurações "**TextColor**" vindas do estilo. Uma propriedade definida diretamente no elemento visual é chamado às vezes de configuração local ou configuração manual, e ela sempre sobrescreve a propriedade configurada pelo **estilo**.

 O objeto "**Style**" no programa "**BasicStyle**" é compartilhado entre os três botões. O compartilhamento de estilos tem uma aplicação importante para os objetos "**Setter**". Qualquer objeto definido para a propriedade "**Value**" de um "**Setter**" deve ser compartilhável. Não tente fazer algo parecido com isto:

![](/assets/12-15-Invalido.png)

Este "XAML" não funciona por duas razões: Conteúdo não é apoiado por uma "**BindableProperty**" e portanto não pode ser utilizado em um "**Setter**". Mas a intenção óbvia aqui é para cada "**Frame**", ou pelo menos para cada estrutura em que este estilo é aplicado, obter esse mesmo objeto "**Label**" como conteúdo. Um objeto "**Label**" único não pode aparecer em vários lugares na página. Uma maneira muito melhor de fazer algo parecido com isto é derivar uma classe de "**Frame**" e definir uma "**Label**" como a propriedade de conteúdo, ou derivar uma classe de "**ContentView**" que inclui um "**Frame**" e um "**Label**".

Você pode querer usar um estilo para definir um manipulador de eventos para um evento como um "**click**". Isso seria útil e conveniente, mas não é suportado. Manipuladores de eventos deve ser definidos nos próprios elementos. \(No entanto, a classe "**Style**" faz objetos de apoio chamados **gatilhos**, que podem responder a eventos ou mudanças de propriedade. Gatilhos serão discutidos em um capítulo futuro.\)

Você não pode definir a propriedade "**GestureRecognizers**" em um estilo. Isso seria útil, mas "**GestureRecognizers**" não é apoiada por uma propriedade de ligação \(bindable\).

Se uma propriedade de ligação é um tipo de referência, e se o valor padrão é **nulo**, você pode usar um estilo para definir a propriedade para um objeto não nulo. Mas você pode também querer sobrescrever esta configuração de estilo com uma configuração local que defina a propriedade de volta para nulo. Você pode definir uma propriedade como nula em "**XAML**" com a extensão de marcação "**{ x: Null }**".

