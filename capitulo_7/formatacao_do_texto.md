## Formatação do Texto {#formata-o-do-texto}

O texto apresentado por um arquivo XAML pode envolver apenas uma ou duas palavras, mas às vezes é necessário um parágrafo inteiro, talvez com alguma formatação de caracteres incorporado. Isso nem sempre é tão óbvio, ou tão fácil, em XAML como pode ser sugerido por nossa familiaridade com HTML.

A solução **TextVariations **tem um arquivo XAML que contém sete Label view para **StackLayout**:

![](/assets/07-50-text)

Cada um dos sete **Label **mostra uma maneira um pouco diferente de definição do texto exibido. Para fins de referência, aqui está o programa em execução em todas as três plataformas:

![Three side-by-side screenshots showing formatted text displayed from XAML on iPhone, Android, and Windows Phone.](../assets/three_side-by-side_screenshots_show.jpeg)

A abordagem mais simples envolve apenas a criação algumas palavras para o atributo de texto do elemento **Label**:

![](/assets/07-51-label)Você também pode definir a propriedade de texto, quebrando a como um elemento da propriedade:![](/assets/07-53-label.text)O texto é a propriedade de conteúdo do rótulo, portanto, você não precisa das tags **Label.Text:**![](/assets/07-54-labe.text)Quando você definir o texto como o conteúdo da **Label**\(se você usar as tags Label.Text ou não\), o texto é cortado: todos os espaços, incluindo tabulações, são removidos do início e no final do texto. No entanto, todos os espaços incorporado é retido, incluindo caracteres de fim de linha.

Quando você definir a propriedade **Text **como um atributo de propriedade, todos os espaços dentro das aspas são mantidos, mas os caracteres de fim de linha são convertidos em espaços.

Resultando em todo um parágrafo de texto formatado de maneira uniforme é um pouco problemático. Você pode colocar todo este parágrafo como uma única linha no arquivo XAML, mas se você preferir usar várias linhas, você deve justificar todo o parágrafo no arquivo XAML entre aspas, da seguinte forma:

![](/assets/07-54-textoformatado)Os caracteres de fim de linha são convertidos em caracteres de espaço para que as linhas individuais sejam devidamente concatenadas. Mas cuidado: Não deixe quaisquer caracteres perdidos no final ou início das linhas individuais. Eles irão aparecer como caracteres estranhos dentro do parágrafo.

Quando várias linhas de texto são especificadas como conteúdo do Label, somente espaços em branco no início e no final do texto é aparado. Todos os espaços em branco incorporados são retido, incluindo caracteres de fim de linha:

![](/assets/07-55-text-label)

Este texto é processado como quatro linhas separadas. Se você está exibindo listas ou poesia em seu aplicativo Xamarin.Forms, isso é exatamente o que você quer. Caso contrário, provavelmente não.

Se a sua linha ou parágrafo do texto requer alguma formatação de parágrafos não uniforme, você vai querer usar a propriedade FormattedText de Label. Como você pode lembrar, em um atribuido FormattedString defina múltiplos objetos Span para a coleção Spans do FormattedString. Em XAML, você precisa de **tags **de property-element para **Label**.**FormattedString**, mas **Spans **é a propriedade de conteúdo de **FormattedString**:![](/assets/07-65-verticaloption)Observe que as propriedades **Text **não formatadas, tem espaços no início ou no fim da cadeia de texto, ou ambos, para que os itens não concatenem ao outro.

No caso geral, no entanto, você pode estar trabalhando com um parágrafo inteiro. Você pode definir o atributo de **Text **de **Span **para uma longa linha, ou você pode envolvê-lo em várias linhas. Tal como acontece com Label, mantenha o bloco inteiro justificado à esquerda no arquivo XAML:![](/assets/07-57-textoformatodo01)Você vai notar no screen shot que o texto com o tamanho da fonte grande está alinhado com o texto normal na linha de base, que é a abordagem adequada tipograficamente, e o espaçamento entre linhas é ajustado para acomodar o texto maior.

Na maioria dos programas Xamarin.Forms, nem XAML nem código existem isoladamente, mas trabalham em conjunto. Elementos em XAML pode desencadear eventos tratados no código, e código podem modificar elementos em XAML. No próximo capítulo, você vai ver como isso funciona.

