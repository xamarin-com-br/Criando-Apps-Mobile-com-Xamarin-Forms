## Acessando membros estáticos {#acessando-membros-est-ticos}

Uma das implementações mais simples e úteis de ImarkupExtension está encapsulada na classe StaticExtension. Isto é parte da especificação XAML original, de modo que habitualmente aparece em XAML com um prefixo x. StaticExtension define uma única propriedade denominada Member do tipo string que você atribui a uma classe e membro nome de uma constante pública, propriedade estática, campo estático, ou enumeração. Vamos ver como isso funciona. Aqui está uma Label com seis propriedades definidas como normalmente apareceriam em XAML.

Cinco destes atributos são atribuidos a seqüências de texto que eventualmente fazem referência a várias propriedades, campos estáticos, e membros de enumeração, mas a conversão dessas cadeias de texto ocorre através de conversores de tipo e a análise XAML padrão de tipos de enumeração.

Se você quiser ser mais explícito na definição desses atributos para essas diferentes propriedades, campos estáticos, e membros de enumeração, você pode usar x: StaticExtension dentro de marcas de elemento de propriedade:

Color.Accent é uma propriedade estática. Color.Black e LayoutOptions.Center são campos estáticos. FontAttributes.Italic e TextAlignment.Center são membros de enumeração.

Tendo em vista a facilidade com que estes atributos são definidos com cadeias de texto, utilizar a abordagem StaticExtension inicialmente parece ridículo, mas note que é um mecanismo de propósito geral. Você pode usar qualquer propriedade estática, campo ou membro de enumeração na tag StaticExtension, se seu tipo corresponde ao da propriedade de destino.

Por convenção, as classes que implementam ImarkupExtension incorporam a palavra Extension em seus nomes, mas você pode deixar isso de lado no XAML, por isso que esta extensão de marcação é geralmente chamada de x:Static em vez de x: StaticExtension . A marcação a seguir é ligeiramente mais curta do que o bloco anterior:

E agora, para um grande salto de sintaxe, uma mudança na sintaxe que faz com que as marcas de propriedade de elementos desapareçam e as pegadas encolham consideravelmente. Extensões de marcação XAML quase sempre aparecem com o nome de extensão de marcação e os argumentos dentro de um par de chaves:

Esta sintaxe com as chaves é tão usada em conexão com extensões de marcação XAML que muitos desenvolvedores consideram que extensões de marcação são sinônimo de sintaxe chave. E isso é quase verdade: enquanto chaves sempre sinalizam a presença de uma extensão de marcação XAML, em muitos casos uma extensão de marcação pode aparecer em XAML sem as chaves (como demonstrado anteriormente) e às vezes é conveniente utilizá-los dessa forma.

Observe que não há aspas dentro das chaves. Dentro dessas chaves, regras muito diferentes de sintaxe se aplicam. A propriedade Member da classe StaticExtension não é mais um atributo XML. Em termos de XML, toda a expressão delimitada pelas chaves é o valor do atributo, e os argumentos dentro das chaves aparecem sem aspas.

Assim como os elementos, as extensões de marcação podem ter um atributo ContentProperty. Extensões de marcação que tem apenas uma propriedade, tais como a classe StaticExtension com a sua única propriedade Member invariavelmente marca essa propriedade exclusiva como a propriedade de conteúdo. Para extensões de marcação usando esta sintaxe, isto significa que o nome da propriedade Member e o sinal de igual podem ser removidos:

Esta é a forma comum da extensão de marcação x: Static.

Obviamente, a utilização de x: Static para essas propriedades particulares é desnecessária, mas você pode definir seus próprios membros estáticos de execução constantes de todo o aplicativo, e você pode fazer referência a estes em seus arquivos XAML. Isto é demonstrado no projeto SharedStatics. O projeto SharedStatics contém uma classe chamada AppConstants que define algumas constantes e campos estáticos que podem ser úteis para a formatação do texto:

O arquivo XAML usa então a extensão de marcação x:Static para fazer referência a esses itens. Observe a declaração de namespace XML que associa o prefixo local com o namespace do projeto:

&quot;Extensão de marcação XAML, um aplicativo pode manter uma coleção de configurações de propriedade comum definidos como constantes,

Você pode estar curioso porque cada um dos objetos Span com uma configuração FontAttributes repete a configuração FontSize que é definida na própria Label. De fato, objetos Span não herdam as configurações de Fonte da Label quando outra configuração relacionadas com o tipo de letra é aplicado. E aqui está:

Esta técnica permite que você use essas configurações de propriedade comuns em várias páginas, e se você precisar alterar os valores, você só precisa mudar o arquivo AppSettings.

Também é possível utilizar x:Static com propriedades estáticas e campos definidos nas classes em bibliotecas externas. O exemplo a seguir, denominado SystemStatics, é bastante artificial, ele define o BorderWidth de um Botão igual ao campo estático PI definido na classe Math e usa a propriedade estática Environment.New-Line para quebras de linha no texto. Mas demonstra a técnica.

As classes Math e Environment são ambas definidas no namespace .NET System, assim que uma nova Declaração de namespace XML é necessária para definir um prefixo chamado (por exemplo) sys para System. Note que esta declaração de namespace especifica o namespace CLR como System mas o conjunto como mscorlib, que originalmente era para Microsoft Common Object Library Runtime mas agora está disponível para Multilanguage Standard Common Object Runtime Library:

A borda do botão não aparece no Android a menos que a cor de fundo e cores de borda do botão também estejam setadas para valores não padrão, então algumas marcações adicionais cuidam do problema. Em plataformas iOS, a borda do botão tende sobrepor o texto do botão, então o texto é definido com espaços no início e no fim.

A julgar somente a partir dos recursos visuais, nós realmente temos que acreditar que a largura da borda do botão é cerca de 3,14 de largura, mas as quebras de linha definitivamente funcionam:

A utilização de chaves para as extensões de marcação implica que você não pode exibir o texto cercado por chaves. As chaves neste texto serão substituidas por uma extensão de marcação:

Isso não vai funcionar. Você pode ter chaves em outras partes da cadeia de texto, mas você não pode começar com uma chave esquerda.

Se você realmente precisa, no entanto, você pode garantir que o texto seja substituido por uma extensão XAML começando o texto com uma sequência de escape que consiste em um par de chaves esquerda e direita:

Que irá exibir o texto que deseja.