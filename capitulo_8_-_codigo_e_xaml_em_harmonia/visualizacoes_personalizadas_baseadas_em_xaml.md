## Visualizações personalizadas baseadas em XAML {#visualiza-es-personalizadas-baseadas-em-xaml}

O programa **ScaryColorList** no capítulo anterior listou algumas cores em um StackLayout usando Frame, BoxView, e Label. Mesmo com apenas três cores, a marcação repetitiva estava começando a parecer muito ameaçador. Infelizmente não há nenhuma marcação XAML que duplique os loops for e while do C#, então a sua escolha é usar o código para gerar vários itens semelhantes, ou encontrar a melhor maneira de fazê-lo na marcação.

Neste livro, você vai verá várias maneiras de listar cores em XAML, e, eventualmente, uma maneira muita limpa e elegante. Fazer este trabalho se tornará claro. Mas isso requer mais alguns passos no aprendizado em Xamarin.Forms. Até então, nós estaremos olhando algumas outras abordagens que podem ser úteis em circunstâncias semelhantes.

Uma estratégia é criar uma _view_ personalizada que tem o único propósito de exibir uma cor com um nome e uma caixa colorida. E enquanto estamos nisso, vamos exibir, também, os valores RGB hexadecimais das cores. Você pode então usar essa view personalizada em um arquivo de página XAML para as cores individuais.

Como isso vai ficar em XAML?

Ou uma melhor pergunta seria: Como você gostaria que isso parecesse?

Se a marcação for semelhante com isto, a repetição não é ruim para todos, e não muito pior do que definir explicitamente valor do vetor de Colorem código:

Bem, na verdade ele não vai ficar exatamente desta assim. MyColorView é, obviamente, uma classe personalizada e não faz parte da API Xamarin.Forms. Portanto, ele não pode aparecer no arquivo XAML sem um prefixo de _namespace_ que é definido em uma declaração de _namespace_ XML.

Com esse prefixo XML aplicado, não haverá qualquer confusão sobre esta visão customizada ser parte da API Xamarin.Forms. Então vamos dar um nome mais digno de ColorViewao invés de MyColorView.

Esta classe ColorViewhipotética é um exemplo de uma visão personalizada simples porque consiste unicamente d _views_ existentes, especificamente Label, Frame, e BoxView- organizadas de uma maneira particular usando StackLayout. Xamarin.Forms define uma _view_ projetada especificamente para a finalidade de herdar um conjunto de _views_, chamado de ContentView. Como em ContentPage, ContentViewtem uma propriedade de conteúdo que você pode definir para uma árvore de outras _views_. Você pode definir o conteúdo do ContentViewno código, mas é mais divertido fazê-lo em XAML.

Vamos colocar uma solução chamado **ColorViewList**. Esta solução terá dois conjuntos de XAML e arquivos _code-behind_, o primeiro é uma classe denominada ColorViewListPage, que deriva de ContentPage(como de costume), e o segundo é uma classe chamada ColorView, que deriva de ContentView.

Para criar a classe ColorViewno Visual Studio, utilize o mesmo procedimento de adicionar uma nova página XAML para o projeto ColorViewList: Com o botão direito do mouse no nome do projeto no **Solution Explorer** selecione **Add** &gt; **New Item** no menu de contexto. No diálogo **Add Item**, selecione **Visual C#** &gt; **Code** à esquerda **e Forms Xaml Page**. Digite o nome do ColorView.cs. Mas de imediato, antes de esquecer, vá para o arquivo ColorView.xaml e mude as _tags_ de início e fim do ContentPagepara ContentView. No arquivo ColorView.xaml.cs, altere a classe base para ContentView.

O processo é um pouco mais fácil no Xamarin Studio. A partir do menu de ferramentas do projeto **ColorViewList**, selecione **Add** &gt; **New File**. Na caixa de diálogo **New File**, selecione **Forms** à esquerda **e Forms ContentView Xaml** (não **o Forms ContentPage XAML**). Dê-lhe o nome de ColorView.

Você também vai precisar criar o arquivo XAML e arquivo de _code-behind_ e para a classe ColorViewListPage, como de costume.

O arquivo ColorView.xaml descreve a disposição dos itens de cores individuais, mas sem quaisquer valores de cores reais. Em vez disso, o BoxView e dois modos de exibição da Label são nomes dados:

Em um programa da vida real, você terá tempo mais tarde para ajustar os visuais. Inicialmente, você só quer pegar todas as views nomeadas lá.

Além dos recursos visuais, esta classe ColorViewvai precisar de uma nova propriedade para definir a cor. Esta propriedade deve ser definida no arquivo _code-behind_. A princípio, parece razoável dar a ColorViewuma propriedade chamada Colordo tipo Color(como o trecho XAML anterior com MyColorViewparece sugerir). Mas se essa propriedade foi do tipo Color, como é que o código obtém o nome da cor do que o valor de Color? Não pode.Em vez disso, faz mais sentido definir uma propriedade chamada Colornamedo tipo string. O arquivo code-behind pode então usar a reflexão para obter o campo estático da classe Colorcorrespondente a esse nome.

Mas espere: Xamarin.Forms inclui uma classe pública ColorTypeConverterque o analisador XAML usa para converter um nome de cor de texto como o &quot;Red&quot; ou &quot;Blue&quot; em um valor Color. Por que não aproveitar isso?

Aqui está o arquivo code-behind para ColorView. Ele define uma propriedade ColorNamecom um método de acesso set que define a propriedade de texto do colorNameLabeldo nome da cor, e então usa ColorTypeConverterpara converter o nome para um valor Color. Este valor Coloré então utilizado para definir a propriedade Colorde boxViewe a propriedade de texto do colorValueLabelpara os valores RGB:

A classe ColorViewestá terminada. Agora vamos olhar para ColorViewListPage. O arquivo ColorViewListPage.xaml deve listar várias instâncias de ColorView, por isso precisa de um novo namespace XML com um novo prefixo namespace para referenciar o elemento ColorView.

A classe ColorView faz parte do mesmo projeto que ColorViewListPage. Geralmente, os programadores usam um prefixo namespace XML de um local para estes casos. A nova declaração de namespace aparece no elemento raiz do arquivo XAML (como os outros dois) com o seguinte formato:

No caso geral, uma declaração de namespace XML personalizado para XAML deve especificar um _namespace_ e _Common Language Runtime_ (CLR) - também conhecido como o namespace .NET - e um _assembly_. As palavras-chave para especificá-los são clr-namespace e assembly. Muitas vezes, o namespace CLR é o mesmo que a assembly, como eles são neste caso, mas eles não precisam ser necessariamente. As duas partes estão conectadas por um ponto e vírgula.

Observe que um dois pontos segue clr-namespace, mas um sinal de igual segue assembly. Esta aparente incoerência é deliberada: o formato da declaração de _namespace_ se destina a imitar um URI encontrado no _namespace_ de declarações convencionais, nas quais dois pontos seguem o nome do esquema de URI.

Você usa a mesma sintaxe para fazer referência a objetos em bibliotecas de classes portáteis externas. A única diferença nesses casos é que o projeto também precisa de uma referência a esse PCL externo. (Você verá um exemplo no capítulo 10, &quot;extensões de marcação XAML.&quot;).

O prefixo local é comum para o código no mesmo assembly, e, nesse caso, a parte de assembly não é necessária:

Você pode incluí-lo se você quiser, mas não é necessário. Aqui está o XAML para a classe ColorViewListPage. O arquivo _code-behind_ contém nada além da chamada InitializeComponent:

Esta não é tão tedioso como o exemplo anterior, e demonstra como você pode encapsular visuais em suas próprias classes XAML. Observe que o StackLayouté o filho de um ScrollView, para que a lista pode ser rolada:

No entanto, há um aspecto do projeto ColorViewList que não se qualifica como uma &quot;melhor prática&quot;. É a definição da propriedade ColorName em ColorView. Isto realmente deve ser implementado como um objeto BindableProperty_._ Investigar objetos _bindable_ e propriedades _bindable_ ​​é uma das prioridades e será explorada no capítulo 11.