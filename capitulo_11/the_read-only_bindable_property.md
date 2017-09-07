## The Read-Only Bindable Property {#the-read-only-bindable-property}

Suponha que você está trabalhando com uma aplicação em que é conveniente parágrafo saber o número de palavras o texto que é apresentado por um elemento label. Talvez rápido você gostaria de construir esse direito facilidade em uma classe que deriva de Label. Vamos Chamar esta nova classe CountedLabel .

Até agora, o seu primeiro pensamento deve ser o de definir um objeto chamado bindableproperty verbo- countproperty e um problema clr correspondente chamado wordcount.

Mas espere: ela só faz sentido para esta propriedade wordcount um ser definido de dentro da classe countedlabel. isso significa que a propriedade wordcount clr não deve ter um conjunto assessor público. Deve ser definidos desta forma:

public int WordCount

{

private set { SetValue(WordCountProperty, value); }

get { return (double)GetValue(WordCountProperty); }

}

O assessor get ainda é público, mas o assessor set é particular. Isso é suficiente?

Não exatamente. Apesar do assessor set privado na propriedade CLR, o código externo para CountedLabel ainda pode chamar SetValue com o CountedLabel.WordCountProperty propriedade bindable objeto. Esse tipo de configuração de propriedade deve ser proibido também. Mas como isso pode trabalho se o objeto WordCountProperty é público?

A solução é fazer uma propriedade vinculável somente leitura usando o método BindableProperty.CreateReadOnly. (Como Criar, CreateReadOnly também existe em uma forma genérica.) A própria API Xamarin.Forms define várias propriedades somente leitura bindable, por exemplo, as propriedades Largura e Altura definida por VisualElement.

Veja como você pode fazer uma de sua preferência:

O primeiro passo é chamar BindableProperty.CreateReadOnly com os mesmos argumentos que vinculativo ableProperty.Create. No entanto, o método CreateReadOnly devolve um objeto de BindablePropertyKey em vez de BindableProperty. Definir este objeto como estático e readonly como o BindableProperty, mas fazê-lo ser privado para a classe:

public class CountedLabel : Label

{

static readonly BindablePropertyKey WordCountKey = BindableProperty.CreateReadOnly(&quot;WordCount&quot;, // propertyName

typeof(int), // returnType typeof(CountedLabel), // declaringType

0); // defaultValue

…

}

Não pense deste objeto BindablePropertyKey como uma chave de criptografia ou qualquer coisa assim. É muito mais simples, na verdade, apenas um objeto que é privado para a classe.

O segundo passo é fazer com que um objeto BindableProperty pública usando o BindableProperty propriedade do BindablePropertyKey:

public class CountedLabel : Label

{

…

public static readonly BindableProperty WordCountProperty = WordCountKey.BindableProperty;

…

}

Este objeto BindableProperty é público, mas é um tipo especial de BindableProperty: Ele não pode ser usado em uma chamada SetValue. Tentar fazer isso irá gerar um InvalidOperationException.

No entanto, há uma sobrecarga do método SetValue que aceita um objeto BindablePropertyKey. O acessor set CLR pode chamar SetValue usando este objeto, mas este conjunto de assessor deve ser privado para impedir a propriedade de ser definidas fora da classe:

public class CountedLabel : Label

{

…

public int WordCount

{

private set { SetValue(WordCountKey, value); }

get { return (int)GetValue(WordCountProperty); }

}

…

}

A propriedade WordCount agora pode ser definida a partir da classe CountedLabel, mas quando? Esta classe CountedLabel deriva de label, mas precisa para detectar quando a propriedade de texto foi alterada para que ele possa contar as palavras.

Será que label tem um evento TextChanged? Não, não faz. No entanto, bindableobject implementa a interface INotifyPropertyChanged. Esta é uma interface .NET muito importante, particularmente para utilização de aplicações que implementam a arquitetura Model-View-ViewModel. Mais adiante neste livro que você vai ver como usá-lo em suas próprias classes de dados.

A interface INotifyPropertyChanged é definido no namespace System.ComponentModel assim:

public interface INotifyPropertyChanged

{

event PropertyChangedEventHandler PropertyChanged;

}

Cada classe que deriva de bindableobject dispara automaticamente este evento PropertyChanged sempre que qualquer propriedade Apoiado por uma BindableProperty alterações. O objeto PropertyChangedEventArgs que acompanha este evento inclui uma propriedade do tipo string chamada PropertyName que identifica a propriedade que mudou.

Então, tudo que é necessário é para CountedLabel para anexar um manipulador para o evento PropertyChanged e verificar se há um nome de propriedade de &quot;Texto&quot;. De lá, ele pode usar qualquer técnica que ele quer para calculando uma contagem de palavras. A classe CountedLabel completo usa uma função lambda sobre o evento Changed propriedade. O manipulador chama de Split para quebrar a sequência de caracteres em palavras e ver quantos pedaços resultado. O método Split divide o texto com base em espaços, traços e os travessões (Unicode \ u2014)

public class CountedLabel : Label

{

static readonly BindablePropertyKey WordCountKey = BindableProperty.CreateReadOnly(&quot;WordCount&quot;, // propertyName

typeof(int), // returnType typeof(CountedLabel), // declaringType

0); // defaultValue

public static readonly BindableProperty WordCountProperty = WordCountKey.BindableProperty;

public CountedLabel()

{

// Set the WordCount property when the Text property changes. PropertyChanged += (object sender, PropertyChangedEventArgs args) =&gt;

{

if (args.PropertyName == &quot;Text&quot;)

{

if (String.IsNullOrEmpty(Text))

{

}

else

{

}

}

};

}

WordCount = 0;

WordCount = Text.Split(&#039; &#039;, &#039;-&#039;, &#039;\u2014&#039;).Length;

public int WordCount

{

private set { SetValue(WordCountKey, value); }

get { return (int)GetValue(WordCountProperty); }

}

}

A classe inclui uma diretiva using para o namespace System.ComponentModel para o argumento propriedade- ChangedEventArgs para o manipulador. Watch Out: Xamarin.Forms define uma classe chamada PropertyChangingEventArgs (tempo presente). Isso não é o que você quer para o manipulador PropertyChanged. Você quer PropertyChangedEventArgs (passado).

Porque esta chamada do método Split divide o texto em caracteres em branco, traços e os travessões, você pode supor que CountedLabel será demonstrado com texto que contem alguns traços e os travessões. Isto é verdade. O programa **BaskervillesCount** é uma variação do programa **Baskervilles** do Capítulo 3, exceto que o parágrafo de texto é exibido com um CountedLabel e uma etiqueta regular é incluído para exibir a contagem de palavras:

&lt;ContentPage xmlns=[&quot;http://xamarin.com/schemas/2014/form](http://xamarin.com/schemas/2014/forms)s&quot; xmlns:x=[&quot;http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)&quot; xmlns:toolkit=

&quot;clr-namespace:Xamarin.FormsBook.Toolkit;assembly=Xamarin.FormsBook.Toolkit&quot;

x:Class=&quot;BaskervillesCount.BaskervillesCountPage&quot; Padding=&quot;5, 0&quot;&gt;

&lt;StackLayout&gt;

&lt;toolkit:CountedLabel x:Name=&quot;countedLabel&quot; VerticalOptions=&quot;CenterAndExpand&quot; Text=

&quot;Mr. Sherlock Holmes, who was usually very late in the mornings, save upon those not infrequent

occasions when he was up all night, was seated at the breakfast table. I stood upon the hearth-rug and picked up the stick which our visitor had left behind him the night before. It was a fine, thick piece of wood, bulbous-headed, of the sort which

is known as a &amp;#x201C;Penang lawyer.&amp;#x201D; Just

under the head was a broad silver band, nearly an inch across, &amp;#x201C;To James Mortimer, M.R.C.S., from his friends of the C.C.H.,&amp;#x201D; was engraved upon it, with the date &amp;#x201C;1884.&amp;#x201D; It was just such a stick as the old-fashioned family practitioner used to carry&amp;#x2014;dignified, solid, and reassuring.&quot; /&gt;

&lt;Label x:Name=&quot;wordCountLabel&quot; Text=&quot;???&quot; FontSize=&quot;Large&quot;

VerticalOptions=&quot;CenterAndExpand&quot;

HorizontalOptions=&quot;Center&quot; /&gt;

&lt;/StackLayout&gt;

&lt;/ContentPage&gt;

Esse regular Label é definido no arquivo code-behind:

public partial class BaskervillesCountPage : ContentPage

{

public BaskervillesCountPage()

{

InitializeComponent();

int wordCount = countedLabel.WordCount;

wordCountLabel.Text = wordCount + &quot; words&quot;;

}

}

A contagem de palavras que ele calcula baseia-se no pressuposto de que todos os hifens no texto separadas duas palavras, devem ser contadas como duas palavras cada. Isso não é sempre verdade, claro, mas a contagem de palavras não é tão simples como algoritmicamente este código pode implicar:

Como é que o programa será estruturado se o texto alterado dinamicamente enquanto o programa estava em execução? Nesse caso, seria necessário atualizar a contagem de palavras sempre que a propriedade do objeto WordCount CountedLabel for alterada. Você pode anexar um manipulador PropertyChanged no objeto incontáveis ​​edLabel e verificar a propriedade chamada &quot;WordCount&quot;.

No entanto, tenha cuidado se você tentar definir como um manipulador de eventos a partir de XAML: Esse manipulador será acionado quando a propriedade Text é definida pelo analisador XAML, mas o manipulador de eventos no arquivo code-behind não terá acesso a um objeto Label para exibir uma contagem de palavras, porque esse campo wordCountLabel ainda será definido como nulo. Esta é uma questão que vai aparecer novamente ao trabalhar com controles interativos no Capítulo 15, mas praticamente resolvido quando se trabalha com ligação no Capítulo 16 dados.

Há uma outra variação da propriedade bindable chegando no Capítulo 14: Este é o estabelecimento capaz anexado vinculativo e é muito útil na implementação de certos tipos de layouts.

Enquanto isso, vamos olhar para uma das aplicações mais importantes de propriedades vinculáveis: Estilos.