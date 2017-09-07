## Plataformas específicas do arquivo XAML {#plataformas-espec-ficas-do-arquivo-xaml}

Aqui está o arquivo XAML para um programa chamado **ScaryColorList** que é semelhante a um trecho de XAML que você viu anteriormente. Mas agora a repetição é ainda mais assustadora, porque cada item de cor é cercado por um Frame:

&lt;ContentPage xmlns=&quot;[http://xamarin.com/schemas/2014/forms](http://xamarin.com/schemas/2014/forms)&quot; xmlns:x=&quot;[http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)&quot; x:Class=&quot;ScaryColorList.ScaryColorListPage&quot;&gt;

&lt;ContentPage.Content&gt;

&lt;StackLayout&gt;

&lt;StackLayout.Children&gt;

&lt;Frame OutlineColor=&quot;Accent&quot;&gt;

&lt;Frame.Content&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;&gt;

&lt;StackLayout.Children&gt;

&lt;BoxView Color=&quot;Red&quot; /&gt;

&lt;Label Text=&quot;Red&quot;

VerticalOptions=&quot;Center&quot; /&gt;

&lt;/StackLayout.Children&gt;

&lt;/StackLayout&gt;

&lt;/Frame.Content&gt;

&lt;/Frame&gt;

&lt;Frame OutlineColor=&quot;Accent&quot;&gt;

&lt;Frame.Content&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;&gt;

&lt;StackLayout.Children&gt;

&lt;BoxView Color=&quot;Green&quot; /&gt;

&lt;Label Text=&quot;Green&quot;

VerticalOptions=&quot;Center&quot; /&gt;

&lt;/StackLayout.Children&gt;

&lt;/StackLayout&gt;

&lt;/Frame.Content&gt;

&lt;/Frame&gt;

&lt;Frame OutlineColor=&quot;Accent&quot;&gt;

&lt;Frame.Content&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;&gt;

&lt;StackLayout.Children&gt;

&lt;BoxView Color=&quot;Blue&quot; /&gt;

&lt;Label Text=&quot;Blue&quot;

VerticalOptions=&quot;Center&quot; /&gt;

&lt;/StackLayout.Children&gt;

&lt;/StackLayout&gt;

&lt;/Frame.Content&gt;

&lt;/Frame&gt;

&lt;/StackLayout.Children&gt;

&lt;/StackLayout&gt;

&lt;/ContentPage.Content&gt;

&lt;/ContentPage&gt;

O arquivo code-behind contém apenas uma chamada para InitializeComponent.

Além da marcação repetitiva, este programa tem um problema mais prático: Quando ele roda em iOS, o primeiro item sobrepõe a barra de status. Este problema pode ser corrigido com uma chamada para Device.OnPlatform no construtor da página (como você viu no Capítulo 2). Porque Device.OnPlatform define o preenchimento de propriedade na página e não exige nada no arquivo XAML, que poderia ir antes ou depois da chamada do InitializeComponent.

Aqui está uma maneira de fazê-lo:

public partial class ScaryColorListPage : ContentPage

{

public ScaryColorListPage()

{

Padding = Device.OnPlatform(new Thickness(0, 20, 0, 0),

new Thickness(0),

new Thickness(0));

InitializeComponent();

}

}

Ou, você pode definir um valor de preenchimento uniforme para todas as três plataformas à direita no elemento raiz do arquivo XAML:

&lt;ContentPage xmlns=&quot;[http://xamarin.com/schemas/2014/forms](http://xamarin.com/schemas/2014/forms)&quot; xmlns:x=&quot;[http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)&quot; x:Class=&quot;ScaryColorList.ScaryColorListPage&quot; Padding=&quot;0, 20, 0, 0&quot;&gt;

&lt;ContentPage.Content&gt;

…

&lt;/ContentPage.Content&gt;

&lt;/ContentPage&gt;

Que define a propriedade Padding para a página. A classe ThicknessTypeConverter requer que os valores sejam separados por vírgulas, mas você tem a mesma flexibilidade que com o construtor Thickness. Você pode especificar quatro valores na ordem esquerda, superior, direita e inferior; dois valores (o primeiro para esquerda e direita, e o segundo para cima e em baixo); ou um valor.

No entanto, você também pode especificar valores específicos da plataforma direito no arquivo XAML usando a classe OnPlatform, cujo nome sugere que é semelhante a função estática do método Device.OnPlatform.

OnPlatform é uma classe muito interessante, e vale a pena ter uma noção de como ele funciona. a classe é genérica, e tem três propriedades do tipo T, bem como uma conversão implícita de T faz uso do valor Device.OS:

public class OnPlatform&lt;T&gt;

{

public T iOS { get; set; } public T Android { get; set; } public T WinPhone { get; set; }

public static implicit operator T(OnPlatform&lt;T&gt; onPlatform)

{

// returns one of the three properties based on Device.OS

}

}

Em teoria, você pode usar a classe OnPlatform&lt;T&gt; assim como o construtor de um derivado ContentPage:

Padding = new OnPlatform&lt;Thickness&gt;

{

iOS = new Thickness(0, 20, 0, 0), Android = new Thickness(0), WinPhone = new Thickness(0)

};

Você pode configurar uma instância dessa classe diretamente para a propriedade Padding porque a classe OnPlatform define uma conversão implícita de si para o argumento genérico (neste caso Thickness).

No entanto, você não deve usar OnPlatform no código. Use Device.OnPlatform que é projetado para XAML, e a única parte realmente difícil é descobrir como especificar o argumento de tipo genérico.

Felizmente, a especificação XAML 2009 inclui um atributo projetado especificamente para classes genéricas, chamados TypeArguments. Porque é parte da própria XAML, ele é usado com um prefixo de x, então ele aparece como x:TypeArguments. Veja como OnPlatform é usado para selecionar entre três valores de Thickness:

&lt;OnPlatform x:TypeArguments=&quot;Thickness&quot;

iOS=&quot;0, 20, 0, 0&quot;

Android=&quot;0&quot;

WinPhone=&quot;0&quot; /&gt;

Neste exemplo (e no exemplo de código anterior), as configurações do Android e do WinPhone não são necessárias porque elas são os padrões. Note que as cadeias de Thickness podem ser definidas diretamente na propriedades porque essas propriedades são do tipo de Thickness, e, consequentemente, o analisador XAML usará o ThicknessTypeConverter para converte-las.

Agora que temos a marcação OnPlatform, como podemos defini-lo para a propriedade de Padding da Página? Ao expressar Padding usando a sintaxe do property-element, é claro!

&lt;ContentPage xmlns=&quot;[http://xamarin.com/schemas/2014/forms](http://xamarin.com/schemas/2014/forms)&quot; xmlns:x=&quot;[http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)&quot; x:Class=&quot;ScaryColorList.ScaryColorListPage&quot;&gt;

&lt;ContentPage.Padding&gt;

&lt;OnPlatform x:TypeArguments=&quot;Thickness&quot;

iOS=&quot;0, 20, 0, 0&quot; /&gt;

&lt;/ContentPage.Padding&gt;

&lt;ContentPage.Content&gt;

…

&lt;/ContentPage.Content&gt;

&lt;/ContentPage&gt;

Esta é a forma como o programa **ScaryColorList** aparece na coleção de amostras deste livro e aqui como fica:

G07xx02: Três screenshots lado a lado, mostrando uma lista curta de cor XAML no iPhone, Android, e Windows Phone.

![Three side-by-side screenshots showing a short color list from XAML on iPhone, Android, and Windows Phone.](../assets/three_side-by-side_screenshots_show.jpeg)

Semelhante a OnDevice, OnIdiom distingue entre telefone e tablet. Por razões que se tornarão aparente no próximo capítulo, você deve tentar restringir o uso de OnDevice e OnIdiom para pequenos pedaços de marcação em vez de grandes blocos. Ele não deve se tornar um elemento estrutural em seus arquivos XAML.