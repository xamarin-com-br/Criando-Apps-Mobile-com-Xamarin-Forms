## Suporte

XAML

 {#suporte-xaml}

Xamarin.Forms também suporta XAML (pronuncia-se &quot;zammel&quot; para rimar com &quot;camelo&quot;), a baseada em XML Extensible Application Markup Language desenvolvido na Microsoft como uma linguagem de marcação de uso geral para instanciar e inicializar objetos. XAML não se limita a definição de layouts iniciais de interfaces de usuário, mas historicamente, é assim que tem sido o mais utilizado, e é isso que ele é usado no Xamarin.Forms.Here’s the XAML file for the program whose screenshots you’ve just seen:

&lt;ContentPage xmlns=&quot;[http://xamarin.com/schemas/2014/forms](http://xamarin.com/schemas/2014/forms)&quot; xmlns:x=&quot;[http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)&quot; x:Class=&quot;PlatformVisuals.PlatformVisualsPage&quot;

Title=&quot;Visuals&quot;&gt;

&lt;StackLayout Padding=&quot;10,0&quot;&gt;

&lt;Label Text=&quot;Hello, Xamarin.Forms!&quot; VerticalOptions=&quot;CenterAndExpand&quot; HorizontalOptions=&quot;Center&quot; /&gt;

&lt;Button Text = &quot;Click Me!&quot;

VerticalOptions=&quot;CenterAndExpand&quot; HorizontalOptions=&quot;Center&quot; /&gt;

&lt;Switch VerticalOptions=&quot;CenterAndExpand&quot; HorizontalOptions=&quot;Center&quot; /&gt;

&lt;Slider VerticalOptions=&quot;CenterAndExpand&quot; /&gt;

&lt;/StackLayout&gt;

&lt;ContentPage.ToolbarItems&gt;

&lt;ToolbarItem Text=&quot;edit&quot; Order=&quot;Primary&quot;&gt;

&lt;ToolbarItem.Icon&gt;

&lt;OnPlatform x:TypeArguments=&quot;FileImageSource&quot; iOS=&quot;edit.png&quot; Android=&quot;ic_action_edit.png&quot; WinPhone=&quot;Images/edit.png&quot; /&gt;

&lt;/ToolbarItem.Icon&gt;

&lt;/ToolbarItem&gt;

&lt;ToolbarItem Text=&quot;search&quot; Order=&quot;Primary&quot;&gt;

&lt;ToolbarItem.Icon&gt;

&lt;OnPlatform x:TypeArguments=&quot;FileImageSource&quot; iOS=&quot;search.png&quot; Android=&quot;ic_action_search.png&quot; WinPhone=&quot;Images/feature.search.png&quot; /&gt;

&lt;/ToolbarItem.Icon&gt;

&lt;/ToolbarItem&gt;

&lt;ToolbarItem Text=&quot;refresh&quot; Order=&quot;Primary&quot;&gt;

&lt;ToolbarItem.Icon&gt;

&lt;OnPlatform x:TypeArguments=&quot;FileImageSource&quot; iOS=&quot;reload.png&quot; Android=&quot;ic_action_refresh.png&quot; WinPhone=&quot;Images/refresh.png&quot; /&gt;

&lt;/ToolbarItem.Icon&gt;

&lt;/ToolbarItem&gt;

&lt;ToolbarItem Text=&quot;explore&quot; Order=&quot;Secondary&quot; /&gt;

&lt;ToolbarItem Text=&quot;discover&quot; Order=&quot;Secondary&quot; /&gt;

&lt;ToolbarItem Text=&quot;evolve&quot; Order=&quot;Secondary&quot; /&gt;

&lt;/ContentPage.ToolbarItems&gt;

&lt;/ContentPage&gt;

A menos que você tem experiência com XAML, alguns detalhes de sintaxe pode ser um pouco obscura. (Não se preocupe, você vai aprender tudo sobre eles mais tarde neste livro.) Mas, mesmo assim, você pode ver as etiquetas etiqueta, botão, switch e Slider. Em um programa real, o botão, o interruptor e Slider provavelmente teria evento manipuladores anexados que deveriam ser implementadas em um arquivo de código C #. Aqui eles não. Os VerticalOptions e HorizontalOptions atributos ajudar no layout; eles são discutidos no próximo capítulo.