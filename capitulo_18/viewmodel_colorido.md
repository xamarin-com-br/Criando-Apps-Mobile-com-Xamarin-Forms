## ViewModel Colorido {#viewmodel-colorido}

Cores sempre fornecem um bom meio de explorar as características de uma interface gráfica de usuário, então você provavelmente não vai se surpreender ao saber que a biblioteca Xamarin.FormsBook.Toolkit contém uma classe chamada ColorViewModel.

A classe ColorViewModel expõe uma propriedade de cor, mas também vermelho, verde, azul, alfa, matiz, saturação e luminosidade propriedades, todos os quais são individualmente ajustáveis. Esta não é uma característica que a estrutura Xamarin.Form de cores oferece. Uma vez que um valor Cor é criado a partir de um construtor ou um dos métodos em cores que começam com as palavras Adicionar, From, Multiply, ou com, é imutável.

Esta classe ColorViewModel é complicada pela inter-relação dos seus bens a cores e todas as propriedades do componente. Por exemplo, suponha que a propriedade de cores está definida. A classe deve disparar um manipulador dades ertyChanged não só para a cor, mas também para qualquer componente (como o vermelho ou Hue), que também muda. Da mesma forma, se as alterações de propriedade vermelhas, em seguida, a classe deve disparar um evento PropertyChanged para ambos vermelho e cores, e, provavelmente, Hue, Saturação e Luminosidade também.

A classe ColorViewModel resolve este problema armazenando um campo de apoio para apenas a propriedade Color. Todos os assessores estabelecidos para os componentes individuais criar uma nova cor usando o valor de entrada com uma chamada para Color.FromRgba ou Color.FromHsla. Este novo valor Cor está definido para a propriedade de cores em vez do campo de cor, o que significa que o novo valor Cor é submetido a processamento na acessor set da propriedade Cor:

public class ColorViewModel : INotifyPropertyChanged {

Color color;

public event PropertyChangedEventHandler PropertyChanged;

public double Red

{

set

{

if (Round(color.R) != value)

}

Color = Color.FromRgba(value, color.G, color.B, color.A);

get

{

}

return Round(color.R);

}

public double Green

{

set

{

if (Round(color.G) != value)

Color = Color.FromRgba(color.R, value, color.B, color.A);

}

get

{

return Round(color.G);

}

}

public double Blue

{

set

{

if (Round(color.B) != value)

Color = Color.FromRgba(color.R, color.G, value, color.A);

}

get

{

return Round(color.B);

}

}

public double Alpha

{

set

{

}

if (Round(color.A) != value)

Color = Color.FromRgba(color.R, color.G, color.B, value);

get

{

}

return Round(color.A);

}

public double Hue

{

set

{

if (Round(color.Hue) != value)

Color = Color.FromHsla(value, color.Saturation, color.Luminosity, color.A);

}

get

{

return Round(color.Hue);

}

}

public double Saturation

{

set

{

if (Round(color.Saturation) != value)

Color = Color.FromHsla(color.Hue, value, color.Luminosity, color.A);

}

get

{

return Round(color.Saturation);

}

}

public double Luminosity

{

set

{

if (Round(color.Luminosity) != value)

Color = Color.FromHsla(color.Hue, color.Saturation, value, color.A);

}

get

{

return Round(color.Luminosity);

}

}

public Color Color

{

set

{

Color oldColor = color;

if (color != value)

{

color = value;

OnPropertyChanged(&quot;Color&quot;);

}

if (color.R != oldColor.R)

OnPropertyChanged(&quot;Red&quot;);

if (color.G != oldColor.G)

OnPropertyChanged(&quot;Green&quot;);

if (color.B != oldColor.B)

OnPropertyChanged(&quot;Blue&quot;);

if (color.A != oldColor.A)

OnPropertyChanged(&quot;Alpha&quot;);

if (color.Hue != oldColor.Hue)

OnPropertyChanged(&quot;Hue&quot;);

if (color.Saturation != oldColor.Saturation)

OnPropertyChanged(&quot;Saturation&quot;);

if (color.Luminosity != oldColor.Luminosity)

}

OnPropertyChanged(&quot;Luminosity&quot;);

get

{

return color;

}

}

protected void OnPropertyChanged(string propertyName)

{

PropertyChangedEventHandler handler = PropertyChanged;

if (handler != null)

{

handler(this, new PropertyChangedEventArgs(propertyName));

}

}

double Round(double value)

{

return Device.OnPlatform(value, Math.Round(value, 3), value);

}

}

O “accessor” definido para a propriedade cor é responsável pelos disparos de todos os eventos PropertyChanged com base em alterações nas propriedades.

Observe o método dependente do dispositivo na parte inferior da classe e seu uso no conjunto e obter assessores das sete primeiras propriedades. Este foi adicionada quando a amostra MultiColorSliders no Capítulo 23, &quot;disparadores e comportamentos,&quot; revelou um problema. Android parecia ser arredondar internamente os componentes de cor, provocando inconsistências entre as propriedades de serem passados para os métodos e Color.FromRgba Color.FromHsla e as propriedades do valor de cor resultante, que levam ao infinito definir e obter laçadas.

O programa HslSliders instancia o ColorViewModel entre as tags Grid.BindingContext para que se torne o BindingContext para todos os elementos deslizante e etiqueta dentro da grade:

&lt;ContentPage xmlns=&quot;http://xamarin.com/schemas/2014/forms&quot;

xmlns:x=&quot;http://schemas.microsoft.com/winfx/2009/xaml&quot;

xmlns:toolkit=

&quot;clr-namespace:Xamarin.FormsBook.Toolkit;assembly=Xamarin.FormsBook.Toolkit&quot;

x:Class=&quot;HslSliders.HslSlidersPage&quot;

SizeChanged=&quot;OnPageSizeChanged&quot;&gt;

&lt;ContentPage.Padding&gt;

&lt;OnPlatform x:TypeArguments=&quot;Thickness&quot;

iOS=&quot;0, 20, 0, 0&quot; /&gt;

&lt;/ContentPage.Padding&gt;

&lt;Grid x:Name=&quot;mainGrid&quot;&gt;

&lt;Grid.BindingContext&gt;

&lt;toolkit:ColorViewModel Color=&quot;Gray&quot; /&gt;

&lt;/Grid.BindingContext&gt;

&lt;Grid.Resources&gt;

&lt;ResourceDictionary&gt;

&lt;Style TargetType=&quot;Label&quot;&gt;

&lt;Setter Property=&quot;FontSize&quot; Value=&quot;Large&quot; /&gt;

&lt;Setter Property=&quot;HorizontalTextAlignment&quot; Value=&quot;Center&quot; /&gt;

&lt;/Style&gt;

&lt;/ResourceDictionary&gt;

&lt;/Grid.Resources&gt;

&lt;!-- Initialized for portrait mode. --&gt;

&lt;Grid.RowDefinitions&gt;

&lt;RowDefinition Height=&quot;*&quot; /&gt;

&lt;RowDefinition Height=&quot;Auto&quot; /&gt;

&lt;/Grid.RowDefinitions&gt;

&lt;Grid.ColumnDefinitions&gt;

&lt;ColumnDefinition Width=&quot;*&quot; /&gt;

&lt;ColumnDefinition Width=&quot;0&quot; /&gt;

&lt;/Grid.ColumnDefinitions&gt;

&lt;BoxView Color=&quot;{Binding Color}&quot;

Grid.Row=&quot;0&quot; Grid.Column=&quot;0&quot; /&gt;

&lt;StackLayout x:Name=&quot;controlPanelStack&quot;

Grid.Row=&quot;1&quot; Grid.Column=&quot;0&quot;

Padding=&quot;10, 5&quot;&gt;

&lt;StackLayout VerticalOptions=&quot;CenterAndExpand&quot;&gt;

&lt;Slider Value=&quot;{Binding Hue}&quot; /&gt;

&lt;Label Text=&quot;{Binding Hue, StringFormat=&#039;Hue = {0:F2}&#039;}&quot; /&gt;

&lt;/StackLayout&gt;

&lt;StackLayout VerticalOptions=&quot;CenterAndExpand&quot;&gt;

&lt;Slider Value=&quot;{Binding Saturation}&quot; /&gt;

&lt;Label Text=&quot;{Binding Saturation, StringFormat=&#039;Saturation = {0:F2}&#039;}&quot; /&gt;

&lt;/StackLayout&gt;

&lt;StackLayout VerticalOptions=&quot;CenterAndExpand&quot;&gt;

&lt;Slider Value=&quot;{Binding Luminosity}&quot; /&gt;

&lt;Label Text=&quot;{Binding Luminosity, StringFormat=&#039;Luminosity = {0:F2}&#039;}&quot; /&gt;

&lt;/StackLayout&gt;

&lt;/StackLayout&gt;

&lt;/Grid&gt;

&lt;/ContentPage&gt;

Observe que a propriedade de cor de ColorViewModel é inicializado quando ColorViewModel é instanciado. Os dois sentidos ligações dos controles deslizantes em seguida, pegar os valores resultantes das propriedades Hue, Saturação e Luminosidade.

Se você, em vez querer implementar uma exibição de valores hexadecimais de vermelho, verde e azul, você pode usar a classe DoubleToIntConverter demonstrado em conexão com o programa GridRgbSliders no capítulo anterior.

O programa HslSliders implementa a mesma técnica para alternar entre retrato e modos scape terrestres como esse programa GridRgbSliders. O arquivo code-behind lida com a mecânica deste switch:

public partial class HslSlidersPage : ContentPage {

public HslSlidersPage()

{

{

InitializeComponent();

}

void OnPageSizeChanged(object sender, EventArgs args)

// Portrait mode.

if (Width &lt; Height)

{

mainGrid.RowDefinitions[1].Height = GridLength.Auto;

Grid.SetRow(controlPanelStack, 1);

Grid.SetColumn(controlPanelStack, 0);

}

// Landscape mode.

else

{

mainGrid.RowDefinitions[1].Height = new GridLength(0, GridUnitType.Absolute);

mainGrid.ColumnDefinitions[1].Width = new GridLength(1, GridUnitType.Star);

}

mainGrid.ColumnDefinitions[1].Width = new GridLength(0, GridUnitType.Absolute);

Grid.SetRow(controlPanelStack, 0);

Grid.SetColumn(controlPanelStack, 1);

}

}

Este arquivo de código subjacente não é tão bonito quanto um arquivo que apenas pede InitializeComponent, mas, mesmo no contexto de MVVM, alternar entre os modos retrato e paisagem é um uso legítimo do código-behind arquivo porque ele é exclusivamente dedicado ao interface com o usuário, em vez de lógica de negócios subjacente.

Aqui está o programa Hsl Sliders em ação: