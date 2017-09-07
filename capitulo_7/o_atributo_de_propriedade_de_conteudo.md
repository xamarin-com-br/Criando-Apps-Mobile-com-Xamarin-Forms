## O atributo de propriedade de conteúdo {#o-atributo-de-propriedade-de-conte-do}

O arquivo XAML no programa **ScaryColorList** é realmente um pouco mais do que ele precisa ser. você pode eliminar as labels ContentPage.Content, todas as tags StackLayout.Children, e todas as tag Frame.Content, e o programa irá funcionar da mesma forma:

&lt;ContentPage xmlns=&quot;[http://xamarin.com/schemas/2014/forms](http://xamarin.com/schemas/2014/forms)&quot; xmlns:x=&quot;[http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)&quot; x:Class=&quot;ScaryColorList.ScaryColorListPage&quot;&gt;

&lt;ContentPage.Padding&gt;

&lt;OnPlatform x:TypeArguments=&quot;Thickness&quot;

iOS=&quot;0, 20, 0, 0&quot; /&gt;

&lt;/ContentPage.Padding&gt;

&lt;StackLayout&gt;

&lt;Frame OutlineColor=&quot;Accent&quot;&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;&gt;

&lt;BoxView Color=&quot;Red&quot; /&gt;

&lt;Label Text=&quot;Red&quot;

VerticalOptions=&quot;Center&quot; /&gt;

&lt;/StackLayout&gt;

&lt;/Frame&gt;

&lt;Frame OutlineColor=&quot;Accent&quot;&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;&gt;

&lt;BoxView Color=&quot;Green&quot; /&gt;

&lt;Label Text=&quot;Green&quot;

VerticalOptions=&quot;Center&quot; /&gt;

&lt;/StackLayout&gt;

&lt;/Frame&gt;

&lt;Frame OutlineColor=&quot;Accent&quot;&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;&gt;

&lt;BoxView Color=&quot;Blue&quot; /&gt;

&lt;Label Text=&quot;Blue&quot;

VerticalOptions=&quot;Center&quot; /&gt;

&lt;/StackLayout&gt;

&lt;/Frame&gt;

&lt;/StackLayout&gt;

&lt;/ContentPage&gt;

Parece muito mais limpo agora. O único elemento de propriedade esquerda é para a propriedade Padding de ContentPage.

Tal como acontece com quase toda sintaxe XAML, esta eliminação de alguns elementos de propriedade é apoiada pelas classes subjacentes. Em cada classe usado em XAML é permitido definir uma propriedade como uma propriedade _con- tent_ (às _vezes_ também chamado de propriedade _padrão_ _da_ _classe_). Para esta propriedade de conteúdo, as tags do elemento não são necessárias, e qualquer conteúdo XML dentro das tags de início e fim é automaticamente atribuído a esta propriedade. Muito convenientemente, a propriedade do conteúdo é Content ContentPage, a propriedade de conteúdo de StackLayout é Children, e a propriedade de conteúdo de Frame é content.

Estas propriedades de conteúdo são documentadas, mas você precisa saber para onde olhar. Uma classe específica é propriedade de conteúdo usando o ContentPropertyAttribute. Se esse atributo está ligado a uma classe, ele aparece na documentação on-line Xamarin.Forms API, juntamente com a declaração de classe. É assim que aparece na documentação para ContentPage:

[Xamarin.Forms.ContentProperty(&quot;Content&quot;)] public class ContentPage : Page

Se ler em voz alta, soa um pouco redundante: A propriedade Content é a propriedade de conteúdo de ContentPage.

A declaração para a classe Frame é semelhante:

[Xamarin.Forms.ContentProperty(&quot;Content&quot;)] public class Frame : ContentView

StackLayout não tem um atributo ContentProperty aplicado, mas StackLayout deriva de Layout&lt;View&gt;, e Layout&lt;T&gt; tem um atributo ContentProperty:

[Xamarin.Forms.ContentProperty(&quot;Children&quot;)]

public abstract class Layout&lt;T&gt; : Layout, IViewContainer&lt;T&gt; where T : Xamarin.Forms.View

O atributo ContentProperty é herdado pelas classes que derivam de Layout &lt;T&gt;, então Children é a propriedade de conteúdo de StackLayout.

Certamente, não há nenhum problema se você incluir os elementos de propriedade quando não são necessários, mas na maioria dos casos, eles não serão mais exibidos nos programas de exemplo deste livro.