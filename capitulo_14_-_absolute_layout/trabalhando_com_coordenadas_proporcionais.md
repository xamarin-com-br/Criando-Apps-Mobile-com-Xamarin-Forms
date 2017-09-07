## Trabalhando com coordenadas proporcionais {#trabalhando-com-coordenadas-proporcionais}

Trabalhar com posicionamento proporcional em um AbsoluteLayout pode ser complicado. Às vezes você precisa compensar o cálculo interno com o tamanho que deve levar em consideração. Por exemplo, você pode preferir especificar as coordenadas de moto que um valor de X igual 1 significa que a borda esquerda do elemento seja posicionado na borda direita do AbsoluteLayout, e você precisa converter a coordenada para que o AbsoluteLayout entenda.

Na discussão que se segue, uma coordenada não leva em conta o tamanho de coordenadas em que 1 significa que o elemento está posicionado ao lado de fora da borda direita ou na borda inferior do AbsoluteLayout é ferido como uma coordenada fracionada. O objeto desta seção é desenvolver regras para conversão de um fracionário de coordenadas para um proporcional de coordenadas que você pode usar em um AbsoluteLayout. Está conversão requer que você sabe o tamanho do elemento.

Suponha que você está colocando um elemento em um AbsoluteLayout, com um limite de layout retangular. Vamos restringir essa análise para coordenadas horizontais e tamanhos. O processo é o mesmo para as coordenadas e tamanhos verticais.

Esse elemento deve primeiro obter uma largura de alguma forma. O elemento pode calcular a sua própria largura, ou uma largura em unidades independentes de dispositivo podem ser atribuídas a ele através do LayoutBounds. Mas suponhamos que a flag AbsoluteLayoutFlags.WidthProportional esteja definido, o que significa que a largura é calcula com base no campo Width dos limites da disposição e a largura do AbsoluteLayout.

𝑐ℎ𝑖𝑙𝑑.𝑊𝑖𝑑𝑡ℎ=𝑙𝑎𝑦𝑜𝑢𝑡𝐵𝑜𝑢𝑛𝑑𝑠.𝑊𝑖𝑑𝑡ℎ∗𝑎𝑏𝑠𝑜𝑙𝑢𝑡𝑒𝐿𝑎𝑦𝑜𝑢𝑡.𝑊𝑖𝑑𝑡ℎ

Se a flag AbsoluteLayoutFlags.XProportional também estiver definida, então internamente o AbsoluteLayout calcula as coordenadas para o elemento relativo a si mesmo tomando o tamanho do elemento em conta:

𝑟𝑒𝑙𝑎𝑡𝑖𝑣𝑒𝐶ℎ𝑖𝑙𝑑𝐶𝑜𝑜𝑟𝑑𝑖𝑛𝑎𝑡𝑒.𝑋=(𝑎𝑏𝑠𝑜𝑙𝑢𝑡𝑒𝐿𝑎𝑦𝑜𝑢𝑡.𝑊𝑖𝑑𝑡ℎ−𝑐ℎ𝑖𝑙𝑑.𝑊𝑖𝑑𝑡ℎ)∗𝑙𝑎𝑦𝑜𝑢𝑡𝐵𝑜𝑢𝑛𝑑𝑠.𝑋

Por exemplo, se o AbsoluteLayout tive uma largura de 400, e o elemento possui uma largura de 100, e o LayoutBounds.X for 0.5, então o relativeChildCoordinate.X é calculado como 150\. Isso significa que a borda esquerda do elemento é 150 pixels da borda esquerda do elemento pai. Isso faz com que o elemento seja horizontalmente centralizado com o AbsoluteLayout.

Também é possível calcular a coordenada fracionada do elemento:

𝑓𝑟𝑎𝑐𝑡𝑖𝑜𝑛𝑎𝑙𝐶ℎ𝑖𝑙𝑑𝐶𝑜𝑜𝑟𝑑𝑖𝑛𝑎𝑡𝑒.𝑋=𝑟𝑒𝑙𝑎𝑡𝑖𝑣𝑒𝐶ℎ𝑖𝑙𝑑𝐶𝑜𝑜𝑟𝑑𝑖𝑛𝑎𝑡𝑒.𝑋𝑎𝑏𝑠𝑜𝑙𝑢𝑡𝑒𝐿𝑎𝑦𝑜𝑢𝑡.𝑊𝑖𝑑𝑡ℎ \ abosluteLayout.Width

Isso não e o mesmo que a coordenada proporcional pois a coordenada fracionada do elemento de 1 significa que a borda esquerda do elemento fica fora da extremidade direta do AbsoluteLayout, e portanto, o elemento está fora da superfície do AbsoluteLayout. Para continuar o exemplo, a coordenada fracionada do elemento é 150 dividido por 400 ou 0.375\. A esquerda do elemento é posicionada em (0.375 *400) ou 150 unidades da borda esquerda do AbsoluteLayout.

Vamos reorganizar os termos da formula que calcula os limites do elemento em relação as coordenadas do elemento para resolver layoutBounds.X

𝑙𝑎𝑦𝑜𝑢𝑡𝐵𝑜𝑢𝑛𝑑𝑠.𝑋= 𝑟𝑒𝑙𝑎𝑡𝑖𝑣𝑒𝐶ℎ𝑖𝑙𝑑𝐶𝑜𝑜𝑟𝑑𝑖𝑛𝑎𝑡𝑒.𝑋(𝑎𝑏𝑠𝑜𝑙𝑢𝑡𝑒𝐿𝑎𝑦𝑜𝑢𝑡.𝑊𝑖𝑑𝑡ℎ−cℎ𝑖𝑙𝑑.𝑊𝑖𝑑𝑡ℎ) \ (absoluteLayout.Width – child.Width)

E vamos dividir a parte superior e inferior pela largura do AbsoluteLayout:

𝑙𝑎𝑦𝑜𝑢𝑡𝐵𝑜𝑢𝑛𝑑𝑠.𝑋= fractionalChildCoordinate.X \ (1- (child.Width \ absoluteLayout.Width))

Se você também utiliza a largura proporcional, então essa relação no denominador é layoutBounds.Width:

layoutBounds.X = fractionalChildCoordinates.X \ ( 1- layoutBounds.Width)

E isso muitas vezes é uma formula muito útil, pois permite que você converta de uma coordenada fracionada para uma coordenada proporcional, para ser usada em um retângulo com limites de layout.

No exemplo do ChessboardProportional, quando a coluna é igual a 7, o factionalChildCoordinate.X é 7 dividido pelo número de colunas (8), ou 7 \8\. O denominador é 1 menos 1\8 (a proporção da largura no retângulo), ou 7\8 novamente.

Vejamos um exemplo onde a formula é aplicada em um código para coordenadas fracionadas. O programa ProportionalCoordinateCalc tenta reproduzir esta simples figura usando oito elementos BoxView azuis em um AbsoluteLayout rosa.

A figura possui um aspecto 2:1\. Os pares de retângulos azuis na parte superior e inferior tem uma altura de 0.1 unidades fracionais (em relação à altura do AbsoluteLayout), e estão espaçadas 0.1 unidades do topo e de fundo entre si. Os retângulos verticais azuis parecem estar espaçados e dimensionados de modo semelhante, mas por causa da relação de aspecto (2:1), os retângulos verticais têm uma largura de 0.05 unidades e são espaçados com 0.05 unidades a partir da esquerda e direita, entre si.

O AbsoluteLayout é definido e centralizado no arquivo XAML com o seguinte código:

&lt;ContentPage xmlns=[&quot;http://xamarin.com/schemas/2014/form](http://xamarin.com/schemas/2014/forms)s&quot; xmlns:x=[&quot;http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)&quot; x:Class=&quot;ProportionalCoordinateCalc.ProportionalCoordinateCalcPage&quot;&gt;

&lt;ContentPage.Padding&gt;

&lt;OnPlatform x:TypeArguments=&quot;Thickness&quot; iOS=&quot;5, 25, 5, 5&quot; Android=&quot;5&quot;

WinPhone=&quot;5&quot; /&gt;

&lt;/ContentPage.Padding&gt;

&lt;ContentView SizeChanged=&quot;OnContentViewSizeChanged&quot;&gt;

&lt;AbsoluteLayout x:Name=&quot;absoluteLayout&quot; BackgroundColor=&quot;Pink&quot; HorizontalOptions=&quot;Center&quot;

VerticalOptions=&quot;Center&quot; /&gt;

&lt;/ContentView&gt;

&lt;/ContentPage&gt;

O arquivo code-behind define uma matriz de estruturas de retângulo com as coordenadas fracionárias para cada um dos oito elementos BoxView. Em um loop o programa aplica uma ligeira variação da formula final mostrada a cima. Em vez de um denominador igual a 1 menos o valor de layoutBounds.Width (ou layoutBounds.Height), que utiliza a largura (ou altura) dos limites fracionais, que são o mesmo valor.

public partial class ProportionalCoordinateCalcPage : ContentPage

{

public ProportionalCoordinateCalcPage()

{

InitializeComponent();

Rectangle[] fractionalRects =

{

new Rectangle(0.05, 0.1, 0.90, 0.1), // outer top

new Rectangle(0.05, 0.8, 0.90, 0.1), // outer bottom new Rectangle(0.05, 0.1, 0.05, 0.8), // outer left

new Rectangle(0.90, 0.1, 0.05, 0.8), // outer right

new Rectangle(0.15, 0.3, 0.70, 0.1), // inner top

new Rectangle(0.15, 0.6, 0.70, 0.1), // inner bottom new Rectangle(0.15, 0.3, 0.05, 0.4), // inner left new Rectangle(0.80, 0.3, 0.05, 0.4), // inner right

};

foreach (Rectangle fractionalRect in fractionalRects)

{

Rectangle layoutBounds = new Rectangle

{

// Proportional coordinate calculations.

X = fractionalRect.X / (1 - fractionalRect.Width),

Y = fractionalRect.Y / (1 - fractionalRect.Height),

Width = fractionalRect.Width, Height = fractionalRect.Height

};

absoluteLayout.Children.Add(

new BoxView

{

Color = Color.Blue

},

layoutBounds, AbsoluteLayoutFlags.All);

}

}

void OnContentViewSizeChanged(object sender, EventArgs args)

{

ContentView contentView = (ContentView)sender;

// Figure has an aspect ratio of 2:1.

double height = Math.Min(contentView.Width / 2, contentView.Height); absoluteLayout.WidthRequest = 2 * height; absoluteLayout.HeightRequest = height;

}

}

Aqui está o resultado:

E claro, você pode virar o telefone para o modo paisagem, e você verá o resultado: