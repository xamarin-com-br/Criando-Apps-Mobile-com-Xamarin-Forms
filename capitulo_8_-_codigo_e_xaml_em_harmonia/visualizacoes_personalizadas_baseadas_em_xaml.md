## Visualizações personalizadas baseadas em XAML {#visualiza-es-personalizadas-baseadas-em-xaml}

O programa **ScaryColorList** no capítulo anterior listou algumas cores em um **StackLayout **usando **Frame**, **BoxView**, e **Label**. Mesmo com apenas três cores, a marcação repetitiva estava começando a parecer muito ameaçador. Infelizmente não há nenhuma marcação XAML que duplique os **loops for **e **while **do C\#, então a sua escolha é usar o código para gerar vários itens semelhantes, ou encontrar a melhor maneira de fazê-lo na marcação.

Neste livro, você vai verá várias maneiras de listar cores em XAML, e, eventualmente, uma maneira muita limpa e elegante. Fazer este trabalho se tornará claro. Mas isso requer mais alguns passos no aprendizado em Xamarin.Forms. Até então, nós estaremos olhando algumas outras abordagens que podem ser úteis em circunstâncias semelhantes.

Uma estratégia é criar uma _**view**_** **personalizada que tem o único propósito de exibir uma cor com um nome e uma caixa colorida. E enquanto estamos nisso, vamos exibir, também, os valores **RGB **hexadecimais das cores. Você pode então usar essa view personalizada em um arquivo de página XAML para as cores individuais.

Como isso vai ficar em XAML?

Ou uma melhor pergunta seria: Como você gostaria que isso parecesse?

Se a marcação for semelhante com isto, a repetição não é ruim para todos, e não muito pior do que definir explicitamente valor do vetor de **Color **em código:![](/assets/08-20-stacklauout)Bem, na verdade ele não vai ficar exatamente desta assim. **MyColorView **é, obviamente, uma classe personalizada e não faz parte da API Xamarin.Forms. Portanto, ele não pode aparecer no arquivo XAML sem um prefixo de _**namespace**_ que é definido em uma declaração de _**namespace**_ XML.

Com esse prefixo XML aplicado, não haverá qualquer confusão sobre esta visão customizada ser parte da API Xamarin.Forms. Então vamos dar um nome mais digno de **ColorView **ao invés de **MyColorView**.

Esta classe **ColorView **hipotética é um exemplo de uma visão personalizada simples porque consiste unicamente de _**views**_** **existentes, especificamente **Label**, **Frame**, e **BoxView**- organizadas de uma maneira particular usando **StackLayout**. Xamarin.Forms define uma _**view**_** **projetada especificamente para a finalidade de herdar um conjunto de _**views**_, chamado de **ContentView**. Como em **ContentPage**, **ContentView **tem uma propriedade de conteúdo que você pode definir para uma árvore de outras _**views**_. Você pode definir o conteúdo do **ContentView **no código, mas é mais divertido fazê-lo em XAML.

Vamos colocar uma solução chamado **ColorViewList**. Esta solução terá dois conjuntos de XAML e arquivos _**code-behind**_, o primeiro é uma classe denominada **ColorViewListPage**, que deriva de **ContentPage**\(como de costume\), e o segundo é uma classe chamada **ColorView**, que deriva de **ContentView**.

Para criar a classe ColorView no Visual Studio, utilize o mesmo procedimento de adicionar uma nova página XAML para o projeto ColorViewList: Com o botão direito do mouse no nome do projeto no **Solution Explorer** selecione **Add** &gt; **New Item** no menu de contexto. No diálogo **Add Item**, selecione **Visual C\#** &gt; **Code** à esquerda **e Forms Xaml Page**. Digite o nome do **ColorView.cs**. Mas de imediato, antes de esquecer, vá para o arquivo **ColorView.xaml** e mude as _tags_ de início e fim do **ContentPage **para **ContentView**. No arquivo **ColorView.xaml.cs**, altere a classe base para **ContentView**.

O processo é um pouco mais fácil no Xamarin Studio. A partir do menu de ferramentas do projeto **ColorViewList**, selecione **Add** &gt; **New File**. Na caixa de diálogo **New File**, selecione **Forms** à esquerda **e Forms ContentView Xaml** \(não **o Forms ContentPage XAML**\). Dê-lhe o nome de **ColorView**.

Você também vai precisar criar o arquivo XAML e arquivo de _code-behind_ e para a classe **ColorViewListPage**, como de costume.

O arquivo **ColorView.xaml** descreve a disposição dos itens de cores individuais, mas sem quaisquer valores de cores reais. Em vez disso, o **BoxView **e dois modos de exibição da **Label **são nomes dados:![](/assets/08-40-colorview)Em um programa da vida real, você terá tempo mais tarde para ajustar os visuais. Inicialmente, você só quer pegar todas as views nomeadas lá.

Além dos recursos visuais, esta classe  vai precisar de uma nova propriedade para definir a cor. Esta propriedade deve ser definida no arquivo _code-behind_. A princípio, parece razoável dar a **ColorView **uma propriedade chamada **Color **do tipo Color \(como o trecho XAML anterior com **MyColorView **parece sugerir\). Mas se essa propriedade foi do tipo **Color**, como é que o código obtém o nome da cor do que o valor de **Color**? Não pode.Em vez disso, faz mais sentido definir uma propriedade chamada **Color name **do tipo string. O arquivo code-behind pode então usar a reflexão para obter o campo estático da classe **Color **correspondente a esse nome.

Mas espere: Xamarin.Forms inclui uma classe pública **ColorTypeConverter** que o analisador XAML usa para converter um nome de cor de texto como o "**Red**" ou "**Blue**" em um valor **Color**. Por que não aproveitar isso?

Aqui está o arquivo code-behind para **ColorView**. Ele define uma propriedade **ColorName **com um método de acesso set que define a propriedade de texto do color **NameLabel** do nome da cor, e então usa **ColorTypeConverter **para converter o nome para um valor **Color**. Este valor **Color** é então utilizado para definir a propriedade **Color **de **boxView **e a propriedade de texto do **colorValueLabel **para os valores **RGB**: ![](/assets/08-15-rgb)

A classe **ColorView **está terminada. Agora vamos olhar para **ColorViewListPage**. O arquivo **ColorViewListPage.xaml** deve listar várias instâncias de **ColorView**, por isso precisa de um novo **namespace **XML com um novo prefixo **namespace **para referenciar o elemento **ColorView**.

A classe **ColorView **faz parte do mesmo projeto que **ColorViewListPage**. Geralmente, os programadores usam um prefixo namespace XML de um local para estes casos. A nova declaração de namespace aparece no elemento raiz do arquivo XAML \(como os outros dois\) com o seguinte formato:![](/assets/08-16-formato)No caso geral, uma declaração de namespace XML personalizado para XAML deve especificar um _namespace_ e _Common Language Runtime_ \(CLR\) - também conhecido como o namespace .NET - e um _assembly_. As palavras-chave para especificá-los são clr-namespace e assembly. Muitas vezes, o namespace CLR é o mesmo que a assembly, como eles são neste caso, mas eles não precisam ser necessariamente. As duas partes estão conectadas por um ponto e vírgula.

Observe que um dois pontos segue clr-namespace, mas um sinal de igual segue assembly. Esta aparente incoerência é deliberada: o formato da declaração de _namespace_ se destina a imitar um URI encontrado no _namespace_ de declarações convencionais, nas quais dois pontos seguem o nome do esquema de URI.

Você usa a mesma sintaxe para fazer referência a objetos em bibliotecas de classes portáteis externas. A única diferença nesses casos é que o projeto também precisa de uma referência a esse PCL externo. \(Você verá um exemplo no capítulo 10, "extensões de marcação XAML."\).

O prefixo local é comum para o código no mesmo **assembly**, e, nesse caso, a parte de **assembly **não é necessária:![](/assets/08-16-preix)Para um arquivo XAML em uma PCL, você pode incluir a parte de montagem para fazer referência a algo na mesma base, se desejar, mas não é necessário. Para um arquivo XAML em um SAP, no entanto, você não deve incluir a parte de montagem para fazer referência a uma classe local porque não há montagem associada a um SAP. O código no SAP é, na verdade, parte das montagens de plataforma individuais, e todos têm nomes diferentes.

Você pode incluí-lo se você quiser, mas não é necessário. Aqui está o XAML para a classe ColorViewListPage. O arquivo _code-behind_ contém nada além da chamada InitializeComponent:

![](/assets/08-17-initialize)![](/assets/08-17-initialize2)

Esta não é tão tedioso como o exemplo anterior, e demonstra como você pode encapsular visuais em suas próprias classes XAML. Observe que o StackLayouté o filho de um ScrollView, para que a lista pode ser rolada:

![](/assets/08-18-telas)

No entanto, há um aspecto do projeto ColorViewList que não se qualifica como uma "**melhor prática**". É a definição da propriedade **ColorName** em **ColorView**. Isto realmente deve ser implementado como um objeto **BindableProperty**_._ Investigar objetos _bindable_ e propriedades _**bindable**_ ​​é uma das prioridades e será explorada no capítulo 11.

