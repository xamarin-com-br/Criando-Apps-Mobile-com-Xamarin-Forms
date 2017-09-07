## Barras de ferramentas e seus ícones {#barras-de-ferramentas-e-seus-cones}

Um dos principais usos de bitmaps na interface do usuário é a barra de ferramentas Xamarin.Forms, que aparece na parte superior da página no iOS e dispositivos Android e, na parte inferior da página em dispositivos Windows Phone. Itens da barra de ferramentas são tappable e fogo clicado eventos bem como Button.

Não há classe para si barra de ferramentas. Em vez disso, você adiciona objetos do tipo ToolbarItem ao ToolbarItems propriedade de coleção definida por página.

A classe ToolbarItem não deriva de ver como Etiqueta e Button em vez deriva do elemento por meio de MenuItemBase e MenuItem. (MenuItem é usado somente em conexão com o TableView e não será discutida até o Capítulo 19.) Para definir as características de um item de barra de ferramentas, use as seguintes propriedades:

*   Texto - o texto que pode aparecer (dependendo da plataforma e da Ordem)
*   Icon - um objeto FileImageSource referência a um bitmap do projeto de plataforma
*   Ordem - um membro da enumeração ToolbarItemOrder: Padrão, Primary, ou Secundary

Há também uma propriedade de nome, mas ele só duplica a propriedade de texto e deve ser considerado obsoleto.

A propriedade Ordem governa se o ToolbarItem aparece como uma imagem (primária) ou texto (Secundário). O Windows Phone e Windows 10 plataformas móveis são limitados a quatro itens primários, e tanto a dispositivos iPhone e Android começam a ficar lotada com mais do que isso, então isso é uma limitação razoável. Itens secundários adicionais são apenas texto. No iPhone aparecem por baixo os itens primários; no Android e Windows Phone eles não são vistos na tela até que o usuário toca umas reticências vertical ou horizontal.

A propriedade Icon é crucial para itens primários, ea propriedade de texto é crucial para os itens secundários, mas o tempo de execução do Windows também usa texto para exibir uma dica de texto curto por baixo dos ícones para itens primários.

Quando o ToolbarItem é aproveitado, ele dispara um evento clicado. ToolbarItem também tem comando e CommandParameter propriedades como o Button, mas estes são para fins de ligação de dados e será demonstrado em um capítulo posterior.

A coleção ToolbarItems definido pela Página é do tipo IList &lt;ToolbarItem&gt;. Uma vez que você adicionar um ToolbarItem a esta coleção, as propriedades ToolbarItem não pode ser alterado. As configurações de propriedade em vez disso são usados ​​internamente para construir objetos específicos da plataforma.

Você pode adicionar ToolbarItem objecções ao ContentPage no Windows Phone, mas iOS e Android barras de ferramentas rígidas para a NavigationPage ou para uma página navegada de uma NavigationPage. Felizmente, este requisito não significa que todo o tema da navegação de página precisa ser discutido antes que você pode usar a barra de ferramentas. Instanciar um NavigationPage em vez de um ContentPage envolve simplesmente chamar o construtor NavigationPage com o objeto ContentPage recém-criado na classe App.

O programa ToolbarDemo reproduz a barra de ferramentas que você viu nas imagens no Capítulo 1\. O ToolbarDemoPage deriva ContentPage, mas a classe App passa o objeto ToolbarDemoPage para um construtor NavigationPage:

Isso é tudo que é necessário para obter a barra de ferramentas para trabalhar com iOS e Android, e tem algumas outras implicações também. Um título que você pode definir com a propriedade Título da página é exibida na parte superior das telas iOS e Android, eo ícone do aplicativo também é exibido na tela do Android. Outro resultado do uso NavigationPage é que você não precisa mais para definir algum estofamento na parte superior da tela do iOS. A barra de status está agora fora da faixa da página do aplicativo.

Talvez o aspecto mais difícil de usar ToolbarItem está montando as imagens bitmap para a propriedade ícone. Cada plataforma tem exigências diferentes para a composição de cores e tamanho destes ícones, e cada plataforma tem um pouco diferentes convenções para o imaginário. O ícone padrão para partilhar, por exemplo, é diferente em todas as três plataformas.

Por estas razões, faz sentido para cada um dos projectos de plataformas para ter sua própria coleção de ícones da barra de ferramentas, e é por isso Ícone é do tipo FileImageSource.

Vamos começar com as duas plataformas que oferecem coleções de ícones adequados para ToolbarItem.