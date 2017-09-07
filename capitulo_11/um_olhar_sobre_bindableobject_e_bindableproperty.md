## Um olhar sobre Bindableobject e BindableProperty {#um-olhar-sobre-bindableobject-e-bindableproperty}

A existência de duas classes chamadas bindableobject e BindableProperty é provável que seja um pouco confuso no início. Tenha em mente que bindableobject é muito parecido com Object em que ele serve como uma classe base para uma grande parte da API Xamarin.Forms e, particularmente, para elemento e, portanto, VisualElement.

Bindableobject fornece suporte para objetos do tipo BindableProperty. Um objeto BindableProperty estende uma propriedade CLR. A melhor visão sobre BindableProperty vem quando você cria alguns dos seus próprios - como você estará fazendo antes do final deste capítulo, mas você também pode recolher algum entendimento, explorando as BindableProperty existentes.

Para o começo do Capítulo 7 (&quot;XAML vs. Código&quot;) dois botões foram criados com muitas das mesmas configurações de propriedade, exceto que as propriedades de um botão que foram setados no código usando a sintaxe do objeto de inicialização do C# 3.0 e outro botão foi instanciado e inicializado em XAML.

Aqui está um programa só de códigos semelhantes chamado **PropertySettings** que também cria e inicializa dois botões de duas maneiras diferentes. As propriedades da primeira label são definidas a maneira old-fashioned, enquanto as propriedades da segunda label são definidos com uma técnica mais detalhado:

public class PropertySettingsPage : ContentPage

{

public PropertySettingsPage()

{

Label label1 = new Label();

label1.Text = &quot;Text with CLR properties&quot;;

label1.IsVisible = true;

label1.Opacity = 0.75;

label1.XAlign = TextAlignment.Center; label1.VerticalOptions = LayoutOptions.CenterAndExpand; label1.TextColor = Color.Blue;

label1.BackgroundColor = Color.FromRgb(255, 128, 128);

label1.FontSize = Device.GetNamedSize(NamedSize.Large, new Label());

label1.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;

Label label2 = new Label();

label2.SetValue(Label.TextProperty, &quot;Text with bindable properties&quot;); label2.SetValue(Label.IsVisibleProperty, true); label2.SetValue(Label.OpacityProperty, 0.75); label2.SetValue(Label.XAlignProperty, TextAlignment.Center); label2.SetValue(Label.VerticalOptionsProperty, LayoutOptions.CenterAndExpand); label2.SetValue(Label.TextColorProperty, Color.Blue); label2.SetValue(Label.BackgroundColorProperty, Color.FromRgb(255, 128, 128)); label2.SetValue(Label.FontSizeProperty,

Device.GetNamedSize(NamedSize.Large, new Label()));

label2.SetValue(Label.FontAttributesProperty, FontAttributes.Bold | FontAttributes.Italic);

Content = new StackLayout

{

Children =

{

label1, label2

}

};

}

}

Estas duas formas de definir propriedades são inteiramente consistentes:

No entanto, a sintaxe alternativa parece muito impar. Por exemplo:

label2.SetValue(Label.TextProperty, &quot;Text with bindable properties&quot;);

O que é o método SetValue? SetValue é definida por bindableobject, a partir do qual cada objeto visual deriva. Bindableobject também define um método GetValue.

Esse primeiro argumento para SetValue tem o nome de Label.TextProperty, o que indica que TextProperty é estático, mas apesar de seu nome, não é uma propriedade em tudo. É um campo estático da classe da label. TextProperty também é só de leitura, e é definido na classe Label, algo como isto:

public static readonly BindableProperty TextProperty;

Isso é um objeto do tipo BindableProperty. Claro, pode parecer um pouco perturbador que um campo é nomeado TextProperty, mas ele está lá. Porque é estático, no entanto, que existe independentemente de quaisquer objetos de label que pode ou não existir.

Se você olhar na documentação da classe de label, você verá que ele define 10 propriedades INCLUINDO texto, TextColor, FontSize, FontAttributes, e outros. Você também vai ver 10 correspondentes campos públicos estáticos somente de leitura do tipo BindableProperty com os nomes TextProperty, TextCol- orProperty, FontSizeProperty, FontAttributesProperty, e assim por diante.

Essas propriedades e os campos estão intimamente relacionados. Na verdade, interno à classe de label, a propriedade text CLR é definida como este para fazer referência ao objeto TextProperty correspondente:

public string Text

{

set { SetValue(Label.TextProperty, value); }

get { return (string)GetValue(Label.TextProperty); }

}

Então você vê porque é que o seu aplicativo chamado SetValue em Label.TextProperty é exatamente equivalente a configuração da propriedade do text diretamente, e talvez apenas um pouco mais minúsculo mais rápido!

A definição interna da propriedade do text no label não é informação secreta. Este é o código padrão. Embora qualquer classe possa definir um objeto BindableProperty, apenas uma classe que deriva de BindableObject pode ligar para o SetValue e para os métodos GetValue que realmente implementam a propriedade em uma classe. Conversão é necessária para o método GetValue porque é definida como retornando object.

Todo o trabalho real envolvido com a manutenção da propriedade de text está acontecendo nessas chamadas SetValue e GetValue. Os objetos bindableobject e BindableProperty efetivamente estendem a funcionalidade de propriedades CLR padrão para fornecer formas sistemáticas para:

 Definir propriedades

 Dar às propriedades valores padrões

 Armazenar seus valores atuais

 Fornecer mecanismos para validar valores de propriedade

 Manter a consistência entre as propriedades relacionadas em uma única classe

 Responder a alterações de propriedade

 Disparar notificações quando uma propriedade está prestes a mudar e mudou

 Suportar data binding

 Suportar estilos

 Suportar dynamic resources

A estreita relação de uma propriedade chamada de text com um BindableProperty chamado TextProperty se reflete na maneira que os programadores falam sobre estas propriedades: Às vezes, um programador diz que a propriedade Text é &quot;apoiado por&quot; um TextProperty BindableProperty chamado porque TextProperty fornece suporte para infra-estrutura text. Mas um atalho comum é dizer que text é em si uma &quot;propriedade bindable&quot; e, geralmente, ninguém vai ser confundido.

Nem todas as propriedades Xamarin.Forms é uma propriedade ligável. Nem o Content propriedade de ContentPage nem o Children propriedade de layout &lt;T&gt; é uma propriedade ligável. Das 28 propriedades demultado por VisualElement, 26 são apoiados por propriedades vinculáveis, mas a propriedade Bounds e as propriedades de recursos não são.

A classe Span usado em conexão com FormattedString não deriva de BindableObjeto. Portanto Span não herda SetValue e GetValue, e ele não pode implementar objetos BindableProperty.

Isto significa que a propriedade text de label é apoiado por uma propriedade ligável, mas o text de propriedade de Span não é. Faz alguma diferença?

Claro que faz a diferença! Se bem se lembram o programa **DynamicVsStatic** no capítulo anterior, você descobriu que DynamicResource trabalhou na propriedade de text de label, mas não a propriedade text de Span. Será que DynamicResource só funciona com propriedades vinculáveis?

A suposição é praticamente confirmada pela definição do seguinte método público definido pelo Element:

public void SetDynamicResource(BindableProperty property, string key);

Isto é como a chave do dicionário é anexada a uma propriedade particular de um elemento quando essa propriedade é o destino de uma extensão de marcação DynamicResource, e também permite que você defina uma ligação dinâmica de recursos em uma propriedade no código.

Aqui está a classe de página de uma versão só de código de **DynamicVsStatic** chamado **DynamicVsStaticCode**. É um tanto simplificada de excluir a utilização de um objeto FormattedString e Span mas, caso contrário, imita com bastante precisão como o arquivo XAML anterior é analisado e, em particular, como as propriedades de texto dos elementos do rótulo são definidos pelo analisador XAML:

public class DynamicVsStaticCodePage : ContentPage

{

public DynamicVsStaticCodePage()

{

Padding = new Thickness(5, 0);

// Create resource dictionary and add item. Resources = new ResourceDictionary

{

{ &quot;currentDateTime&quot;, &quot;Not actually a DateTime&quot; }

};

Content = new StackLayout

{

Children =

{

new Label

{

Text = &quot;StaticResource on Label.Text:&quot;, VerticalOptions = LayoutOptions.EndAndExpand,

FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label))

},

new Label

{

Text = (string)Resources[&quot;currentDateTime&quot;],

VerticalOptions = LayoutOptions.StartAndExpand, XAlign = TextAlignment.Center,

FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label))

},

new Label

{

Text = &quot;DynamicResource on Label.Text:&quot;, VerticalOptions = LayoutOptions.EndAndExpand,

FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label))

}

}

};

// Create the final label with the dynamic resource. Label label = new Label

{

VerticalOptions = LayoutOptions.StartAndExpand, XAlign = TextAlignment.Center,

FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label))

};

label.SetDynamicResource(Label.TextProperty, &quot;currentDateTime&quot;); ((StackLayout)Content).Children.Add(label);

// Start the timer going. Device.StartTimer(TimeSpan.FromSeconds(1),

() =&gt;

{

});

}

}Resources[&quot;currentDateTime&quot;] = DateTime.Now.ToString();

return true

A propriedade de text da segunda label é definida diretamente no acesso do dicionário, e faz com que o uso do dicionário pareça um pouco sem sentido neste contexto. Mas a propriedade de text da última label é vinculada à chave do dicionário por meio de uma chamada para SetDynamicResource, que permite a propriedade ser atualizada quando o conteúdo do dicionário mudar:

Considere o seguinte: O que a assinatura deste método SetDynamicResource poderá ser se ele não poderá se referir a uma propriedade usando o objeto BindableProperty? É fácil fazer referência a uma propriedade value em chamadas de método, mas não a própria propriedade. Há um par de maneiras, tais como a classe PropertyInfo no System.Reflection, ou o objeto de expressão LINQ. Mas o objeto BindableProper-ty foi projetado especificamente para este fim, bem como o trabalho essencial de lidar com a ligação subjacente entre a propriedade e a chave do dicionário.

Da mesma forma, quando nós explorarmos estilos no próximo capítulo, vocês encontrarão uma classe Setter usado em conexão com estilos. Setter define uma propriedade denominada Property do tipo BindableProperty, que determina que qualquer propriedade alvo de um estilo deve ser apoiada por uma propriedade ligável. Isto permite que um estilo possa ser definido antes dos elementos visados ​​pelo modelo.

O mesmo vale para as ligações de dados. A classe bindableobject define um método SetBinding que é muito semelhante ao processo definido no elemento SetDynamicResource:

public void SetBinding(BindableProperty targetProperty, BindingBase binding);

Novamente, observe o tipo do primeiro argumento. Qualquer propriedade alvo de uma ligação de dados deve ser apoiada por uma propriedade ligável.

Por estas razões, sempre que você criar uma exibição personalizada e precisar definir propriedades públicas, sua inclinação padrão deve ser defini-las como propriedades vinculáveis. Somente se após cuidadosa consideração de concluir que não é necessário ou apropriado para a propriedade a ser alvo de um estilo ou uma ligação de dados, você deve recuar e definir uma propriedade CLR ordinária em vez disso.

Assim, sempre que você criar uma classe que deriva de bindableobject, uma das primeiras peças de código que você deve estar digitando a classe começa &quot;BindableProperty public static readonly&quot; - talvez a sequência mais característica de quatro palavras em toda a programação Xamarin.Forms.