## Absolute Layout no Código {#absolute-layout-no-c-digo}

Você pode adicionar um elemento filho a coleção (Children) de um AbsoluteLayout. O mesmo é permitido no StackLayout:

**absoluteLayout.Children.Add(child);**

Porém você também tem outras opções. O AbsoluteLayout redefine a propriedade Children para ser do tipo AbsoluteLayout.IAbsoluteList&lt;View&gt;, o qual inclui dois métodos Add, que permite especificar a posição do elemento e opcionalmente seu tamanho.

Para especificar a posição e o tamanho, você utilizará a classe Rectangle. Rectangle é uma estrutura, e você pode criar um Rectangle utilizando o construtor que aceita as classes Point e Size.

Point point = new Point(x, y);

Size size = new Size(width, height);

Rectangle rect = new Rectangle(point, size);

Ou você pode passar os valores de x, y, width e height diretamente para o construtor do Rectangle.

Rectangle rect = new Rectangle(x, y, width, height);

Você pode utilizar o método alternativo Add para adicionar uma View para a coleção de elementos do AbsoluteLayout:

**absoluteLayout.Children.Add(child, rect);**

Os valores de x e y indicam a posição da borda superior esquerda do elemento relativo a borda superior esquerda do AbsoluteLayout principal. Se você preferir que o elemento permaneça com seu tamanho, você pode apenas informar um valor para Point.

**absoluteLayout.Children.Add(child, point);**

Um pequeno exemplo:

public class AbsoluteDemoPage : ContentPage

{

public AbsoluteDemoPage()

{

AbsoluteLayout absoluteLayout = new AbsoluteLayout

{

Padding = new Thickness(50)

};

absoluteLayout.Children.Add(

new BoxView

{

Color = Color.Accent

},

new Rectangle(0, 10, 200, 5));

absoluteLayout.Children.Add(

new BoxView

{

Color = Color.Accent

},

new Rectangle(0, 20, 200, 5));

absoluteLayout.Children.Add(

new BoxView

{

Color = Color.Accent

},

new Rectangle(10, 0, 5, 65));

absoluteLayout.Children.Add(

new BoxView

{

Color = Color.Accent

},

new Rectangle(20, 0, 5, 65));

absoluteLayout.Children.Add(

new Label

{

Text = &quot;Stylish Header&quot;, FontSize = 24

},

new Point(30, 25));

absoluteLayout.Children.Add(

new Label

{

FormattedText = new FormattedString

{

Spans =

{

new Span

{

Text = &quot;Although the &quot;

},

new Span

{

Text = &quot;AbsoluteLayout&quot;,

FontAttributes = FontAttributes.Italic

},

new Span

{

Text = &quot;Label&quot;,

FontAttributes = FontAttributes.Italic

}

}

}

},

new Point(0, 80));

this.Content = absoluteLayout;

}

}

Quatro elementos do tipo BoxView formam uma sobreposição de padrão no topo para remover o cabeçalho, e o parágrafo que o segue. As posições e tamanhos dos elementos BoxView são configuradas enquanto posiciona os dois componentes de Label.

Um pouco de tentativa e erro foi necessário para obter os tamanhos dos quatro elementos de BoxView e o texto do cabeçalho para ser aproximadamente o mesmo tamanho. Mas note que os elementos BoxView sobrepões: AbsoluteLayout permite sobrescrever as views de uma maneira livre, onde não é possível com o StackLayout (ou sem o uso de transformações, que serão explicados em um capitulo posterior).

A grande desvantagem do AbsoluteLayout é que você precisa posicionar as coordenadas manualmente ou calcular conforme o tempo de execução. Tudo que não é explicitamente dimensionado (como os dois elementos de Label), irá ter um tamanho que é obtido até antes da página ser exibida. Se você procura adicionar outro parágrafo após a segunda Label, quais coordenadas você deverá usar?

Atualmente você pode posicionar múltiplos parágrafos de texto utilizando o StackLayout (ou o StackLayout dentro de um ScrollView) em um AbsoluteLayout e então inserir os labels nele. Layouts podem ser aninhados.

Como você pode perceber, utilizar AbsoluteLayout é mais difícil de utilizar do que o StackLayout. Geralmente é muito mais fácil utilizar Xamarin.Forms e outra classe Layout para tratar itens mais complexos de layout. Mas para casos especiais o AbsoluteLayout é ideal.

Como todo elemento visual, o AbsoluteLayout possui as propriedades HorizontalOptions e VerticalOptions para configurar o Fill por padrão, que significa que o AbsoluteLayout preenche o conteúdo. Com outras configurações, um AbsoluteLayout se ajusta para o tamanho do seu conteúdo, mas há algumas exceções: Tentar colocar o AbsoluteLayout no programa de exemplo com uma cor de fundo para que você possa ver exatamente o espaço que ocupa na tela. Ele normalmente preenche toda a página, mas se você definir as propriedades HorizontalOptions e VerticalOptions para Center, você verá que o tamanho do AbsoluteLayout automaticamente calcula e preenche o conteúdo, mas apenas uma linha do parágrafo de texto.

Descobrir tamanhos para elementos visuais em um AbsoluteLayout pode ser complicado. Uma abordagem simples é demonstrada pelo programa ChessboardFixed abaixo. O nome do programa tem o sufixo Fixed pois a posição e o tamanho de todos os quadrados dentro do tabuleiro de xadrez são definidas no construtor.

Observe que o AbsoluteLayout é centralizado e então irá ser o tamanho que acomode todos os seus elementos filhos. O tabuleiro em si é dado com uma cor marrom, e em seguida os 32 elementos BoxView verde escuros são exibidos em todas as outras posições do tabuleiro:

public class ChessboardFixedPage : ContentPage

{

public ChessboardFixedPage()

{

const double squareSize = 35;

AbsoluteLayout absoluteLayout = new AbsoluteLayout

{

BackgroundColor = Color.FromRgb(240, 220, 130), HorizontalOptions = LayoutOptions.Center, VerticalOptions = LayoutOptions.Center

};

for (int row = 0; row &lt; 8; row++)

{

for (int col = 0; col &lt; 8; col++)

{

// Skip every other square. if (((row ^ col) &amp; 1) == 0)

continue;

BoxView boxView = new BoxView

{

Color = Color.FromRgb(0, 64, 0)

};

Rectangle rect = new Rectangle(col * squareSize, row * squareSize,

squareSize, squareSize);

absoluteLayout.Children.Add(boxView, rect);

}

}

this.Content = absoluteLayout;

}

}

Os cálculos sobre as variáveis de linhas e colunas provocam que uma BoxView seja criada, conforme resultado a seguir: