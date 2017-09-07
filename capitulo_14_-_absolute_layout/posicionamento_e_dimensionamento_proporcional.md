## Posicionamento e dimensionamento proporcional {#posicionamento-e-dimensionamento-proporcional}

Como você viu, o programa ChessboardDynamic ajusta os elementos BoxView com cálculos baseados no tamanho no AbsoluteLayout. Em outras palavras, o tamanho e a posição de cada elemento são proporcionais ao tamanho do conteúdo. Curiosamente, esse é um caso com um AbsoluteLayout, e que poderia ser bom se o AbsoluteLayout acomodasse algumas situações automaticamente.

Ele faz!

AbsoluteLayout define uma segunda propriedade anexada, chamado de LayoutFlagsProperty, e mais dois métodos estáticos, chamado SetLayoutFlags e GetLayoutFlags. Configurando a propriedade anexada, permite que você especifique a posição ou tamanho do elemento, que são proporcionais ao tamanho do AbsoluteLayout. Ao inserir os elementos filhos, o AbsoluteLayout escala essas coordenadas e tamanhos de forma adequada.

Você escolhe como esse recurso funciona com um membro do enumerador AbsoluteLayoutFlags:

*   None (igual a 0)
*   XProportional (1)
*   YProportional (2)
*   PositionProportional (3)
*   WidthProportional (4)
*   HeightProportional (8)
*   SizeProportional (12)
*   All (\xFFFFFFFF)

Você pode definir uma posição e tamanho proporcional de um elemento do AbsoluteLayout usando os métodos estáticos:

AbsoluteLayout.SetLayoutBounds(view, rect); AbsoluteLayout.SetLayoutFlags(view, AbsoluteLayoutFlags.All);

Ou você pode usar uma versão do método Add de um elemento que aceita um membro do enumerador AbsoluteLayoutFlags:

absoluteLayout.Children.Add(view, rect, AbsoluteLayoutFlags.All);

Por exemplo, se você usar o SizeProportional e definir a largura de um elemento para 0.25 e a altura para 0.10, o elemento irá ser um quarto da largura do AbsoluteLayout e um décimo da altura.

A flag PositionProporcional é similar, mas ele assume que o tamanho do elemento em conta: A posição de (0, 0) põe o elemento na borda esquerda superior, e a posição (1, 1) põe o elemento na borda inferior direita, e a posição de (0.5, 0.5) centraliza o elemento com o AbsoluteLayout. Tomando o tamanho do elemento em conta é ótimo em algumas tarefas, como centralizar o elemento em um AbsoluteLayout ou exibir ele nos cantos, mas é um pouco ruim é outras tarefas.

Aqui está um tabuleiro proporcional. A maior parte do trabalho de posicionamento foi movido novamente para o construtor. O evento SizeChanged agora apenas mantém o aspecto global definindo as propriedades WidthRequest e HeightRequest do AbsoluteLayout ao mínimo da largura e altura do ContentView.

public class ChessboardProportionalPage : ContentPage

{

AbsoluteLayout absoluteLayout;

public ChessboardProportionalPage()

{

absoluteLayout = new AbsoluteLayout

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

Rectangle rect = new Rectangle(col / 7.0, // x row / 7.0, // y

1 / 8.0, // width

1 / 8.0); // height

absoluteLayout.Children.Add(boxView, rect, AbsoluteLayoutFlags.All);

}

}

ContentView contentView = new ContentView

{

Content = absoluteLayout

};

contentView.SizeChanged += OnContentViewSizeChanged;

this.Padding = new Thickness(5, Device.OnPlatform(25, 5, 5), 5, 5);

this.Content = contentView;

}

void OnContentViewSizeChanged(object sender, EventArgs args)

{

ContentView contentView = (ContentView)sender;

double boardSize = Math.Min(contentView.Width, contentView.Height); absoluteLayout.WidthRequest = boardSize; absoluteLayout.HeightRequest = boardSize;

}

}

A tela se parece exatamente como o programa ChessboardDynamic.

Cada BoxView é adicionado ao AbsoluteLayout com o código a seguir. Todos os denominadores são valores de ponto flutuante, então os resultados das divisões são convertidos para double.

Rectangle rect = new Rectangle(col / 7.0, row / 7.0, 1 / 8.0, 1 / 8.0);

absoluteLayout.Children.Add(boxView, rect, AbsoluteLayoutFlags.All);

A largura e a altura são sempre iguais a um oitavo da largura e altura do AbsoluteLayout. Isso é muito claro. Mas as variáveis de linha e coluna são divididas por 7 (em vez de 9) para mantar a relação das coordenadas x e y. As variáveis de linha e coluna no loop variam de 0 até 7\. Os valores de linha e coluna de 0 correspondem a esquerda ou topo, mas a fileira de valores de coluna de 7 deve mapear as coordenadas para x e y de 1 para a posição do elemento contra a borda direita ou inferior.

Se você acha que pode precisar de algumas regras solidas para derivar coordenadas proporcionais, leia: