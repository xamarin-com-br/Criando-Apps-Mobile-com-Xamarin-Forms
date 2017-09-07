## O Processamento do Clique {#o-processamento-do-clique}

Aqui está um programa chamado **ButtonLogger** com um Button que compartilha uma StackLayout com um ScrollView contendo outro StackLayout. Cada vez que o Button é clicado, o programa adiciona um novo Label para o StackLayout de rolagem, de fato registrando todos os cliques do botão:

Nos programas neste livro, os eventos recebem nomes que começam com a palavra On, seguido por algum tipo de identificação do componente disparador do evento (por vezes apenas o tipo de componente), seguido do nome do evento. O nome resultante neste caso é OnButtonClicked. O construtor atribui o evento Clicked para o Button logo depois que o Button é criado. A página então é montada com um StackLayout contendo o Button e um ScrollView com outro StackLayout, chamado loggerLayout. Observe que o ScrollView tem suas VerticalOptions definidas para FillAndExpand para que ele possa compartilhar o StackLayout com o Button e ainda ser visível e de rolagem.

Aqui está a tela depois de vários cliques do botão:

Como você pode ver, o Button parece um pouco diferente nas três telas. Isso porque o botão é processado de forma nativa em cada plataforma: no iPhone é um UIButton, no Android é um Android Button, e no Windows Phone é um Windows Phone Button. Por padrão, o botão sempre preenche a área disponível para ele e centraliza o texto internamente.

Button define várias propriedades que permitem personalizar sua aparência:

*   FontFamily
*   FontSize
*   FontAttributes
*   TextColor
*   BorderColor
*   BorderWidth
*   BorderRadius
*   Image (a ser discutido no Capítulo 13)

Button também herda a propriedade BackgroundColor (entre tantas outras propriedades) de VisualElement e herda HorizontalOptions e VerticalOptions de View.

Algumas propriedades do Button podem não funcionar em todas as plataformas. No iPhone você precisa definir Border-Width para um valor positivo para uma borda ser exibida, mas isso é normal para um botão de iPhone. O botão Android não vai exibir uma borda a menos que BackgroundColor seja definida, e, em seguida, ele requer uma configuração não-padrão de BorderColor e um BorderWidth positivo. A propriedade BorderRadius se destina a arredondar os cantos afiados da borda, mas não funciona no Windows Phone. Suponha que você escreveu um programa semelhante ao **ButtonLogger** mas não salvou o objeto loggerLayout como um campo. Você poderia obter acesso ao objeto StackLayout no evento Clicked? Sim! É possível obter elementos visuais pai e filho por meio da técnica de navegar pela árvore visual. O argumento sender para o evento OnButtonClicked é o objeto que dispara o evento, neste caso, o Button, para que possa começar o evento Clicked lançando esse argumento:

**Button button = (Button)sender;**

Você sabe que o Button é um nó de um StackLayout, de modo que objeto é acessível a partir da propriedade ParentView. Mais uma vez, é necessário uma conversão:

**StackLayout outerLayout = (StackLayout)button.ParentView;**O segundo nó deste StackLayout é o ScrollView, portanto, a propriedade Children pode ser indexada para conseguir que:

**ScrollView scrollView = (ScrollView)outerLayout.Children[1];**A propriedade Content deste ScrollView é exatamente o StackLayout que você estava procurando:

**StackLayout loggerLayout = (StackLayout)scrollView.Content;**É claro, o perigo em fazer algo parecido com isto é que você pode alterar o layout algum dia e se esqueça de alterar o código de navegação da árvore visual semelhante. Mas a técnica é útil se o código que monta sua página é separado do código de tratamento de eventos da View dessa página.