## Definindo Bindable Properties {#definindo-bindable-properties}

Suponha que você gostaria de uma classe Label avançada que permite que você especifique o tamanho das fontes em unidades de pontos. Vamos chamar essa classe AltLabel de &quot;Label alternativa.&quot; Ela deriva de Label e inclui uma nova propriedade denominada PointSize.

PointSize deve ser apoiada por uma propriedade bindable? Claro! (Embora as vantagens reais de fazê-la não irá ser demonstrada até os próximos capítulos.)

O único código da classe AltLabel está incluído na biblioteca **Xamarin.FormsBook.Toolkit** por isso é acessível a múltiplas aplicações. A nova propriedade PointSize é implementada com um objeto erty BindableProp - chamado PointSizeProperty e uma propriedade CLR chamada PointSize que referencia PointSizeProperty:

public class AltLabel : Label

{

public static readonly BindableProperty PointSizeProperty … ;

…

public double PointSize

{

set { SetValue(PointSizeProperty, value); }

get { return (double)GetValue(PointSizeProperty); }

}

…

}

Ambas as definições de propriedade devem ser públicas.

Porque PointSizeProperty é definido como estática e somente leitura, ele deve ser atribuído quer em um construtor estático ou para a direita definição de campo, após o que não pode ser alterado. Geralmente, um objeto BindableProperty é atribuído na definição de campo utilizando o método BindableProperty.Create estática. Quatro argumentos são necessários (mostrado aqui com os nomes de argumentos):

 propertyName — O nome de texto da propriedade (neste caso &quot;PointSize&quot;)

 returnType — O tipo da propriedade (um duplo neste exemplo)

 declaringType — O tipo da classe que define a propriedade (AltLabel)

 defaultValue — Um valor padrão (digamos 8 pontos)

Geralmente, o segundo e terceiro argumentos são definidos com expressões typeof. Aqui está a atribuição indicação com estes quatro argumentos passados para BindableProperty.Create:

public class AltLabel : Label

{

public static readonly BindableProperty PointSizeProperty = BindableProperty.Create(&quot;PointSize&quot;, // propertyName

typeof(double), // returnType typeof(AltLabel), // declaringType

8.0, // defaultValue

…);

…

}

Observe que o valor padrão é especificado como 8,0, em vez de apenas 8\. Porque BindableProperty.Create é projetado para lidar com propriedades de qualquer tipo, o parâmetro é definido como defaultValue objeto. Quando o compilador C# encontra apenas um conjunto de 8 como sendo este argumento, ele assumirá que o 8 é um int e passa um int para o método. O problema não será revelado até a execução, no entanto, quando o método BindableProperty.Create estará esperando o valor padrão para ser do tipo duplo e responder levantando um TypeInitializationException.

Você deve ser explícito sobre o tipo do valor que você está especificando como padrão. Não fazê-lo é um erro muito comum na definição de propriedades vinculáveis.

BindableProperty.Create também tem seis argumentos opcionais. Aqui estão eles com os nomes de argumentos e sua finalidade:

 defaultBindingMode — usado em ligação com a ligação de dados

 validateValue — um retorno de chamada para verificar se há um valor válido

 propertyChanged — uma chamada de retorno para indicar quando a propriedade foi alterada

 propertyChanging — uma chamada de retorno para indicar quando a propriedade está prestes a mudar

 coerceValue — um retorno de chamada para coagir um valor definido para outro valor (por exemplo, para restringir os valores para um intervalo)

 defaultValueCreator — um retorno de chamada para criar um valor padrão. Esta função é geralmente utilizada para instanciar um objeto padrão que não pode ser compartilhado entre todas as instâncias da classe, por exemplo, um objeto de coleta, tais como a lista ou dicionário.

Não execute qualquer validação, coerção ou de propriedade alterado manipulação na propriedade CLR. A propriedade CLR deve ser restrita a chamadas SetValue e GetValue. Tudo o resto deve ser feito nos retornos proporcionados pela infraestrutura de propriedade bindable.

É muito raro que uma chamada especial para BindableProperty.Create precisaria de todos estes argumentos opcionais. Por essa razão, esses argumentos opcionais são comumente indicados com o recurso argumento nomeado introduzido em C#4.0\. Para especificar um argumento opcional em particular, use o nome do argumento seguido por dois pontos. Por exemplo:

public class AltLabel : Label

{

public static readonly BindableProperty PointSizeProperty = BindableProperty.Create(&quot;PointSize&quot;, // propertyName

typeof(double), // returnType typeof(AltLabel), // declaringType

8.0, // defaultValue

propertyChanged: OnPointSizeChanged);

…

}

Sem dúvida, propertyChanged é o mais importante dos argumentos opcionais porque a classe usa esse retorno de chamada para ser notificado quando as alterações de propriedade, seja diretamente de uma chamada para SetValue ou através da propriedade CLR.

Neste exemplo, o manipulador de propriedade alterada é chamado OnPointSizeChanged. Ele será chamado somente quando a propriedade realmente muda, e não quando é simplesmente definido para o mesmo valor. No entanto, porque OnPointSizeChanged é referenciado a partir de um campo estático, o próprio método deve também ser estático. Aqui está o que parece:

public class AltLabel : Label

{

…

static void OnPointSizeChanged(BindableObject bindable, object oldValue, object newValue)

{

…

}

…

}

Isto parece um pouco estranho. Poderíamos ter várias instâncias AltLabel em um programa, ainda, sempre que a propriedade Pointsize altera em qualquer um desses casos, esse mesmo método estático é chamado. Como o método de saber exactamente qual instância AltLabel mudou?

O método sabe porque é sempre o primeiro argumento. Esse primeiro argumento é, na verdade, do tipo AltLabel, e indica que a propriedade de AltLabel instância mudou. Isso significa que você pode lançar com segurança o primeiro argumento para uma instância AltLabel:

static void OnPointSizeChanged(BindableObject bindable, object oldValue, object newValue)

{

AltLabel altLabel = (AltLabel)bindable;

…

}

Você pode fazer referência alguma coisa no caso particular de AltLabel cuja propriedade foi alterada. Os segundo e terceiro argumentos são realmente do tipo double para este exemplo, e indicar o prévio valor e o novo valor.

Muitas vezes é conveniente para este método estático para chamar um método de instância com os argumentos convertidos com seus tipos reais:

public class AltLabel : Label

{

…

static void OnPointSizeChanged(BindableObject bindable, object oldValue, object newValue)

{

((AltLabel)bindable).OnPointSizeChanged((double)oldValue, (double)newValue);

}

void OnPointSizeChanged(double oldValue, double newValue)

{

…

}

}

O método de instância pode então fazer uso de quaisquer propriedades de instância ou métodos da classe base subjacente, tal como faria normalmente.

Para esta classe, este método OnPointSizeChanged precisa definir a propriedade FontSize com base em o novo tamanho de ponto e um fator de conversão depende do dispositivo. Além disso, o construtor tem de iniciar o funcionamento da propriedade TamanhoDoTipoDeLetra com base no valor PointSize padrão. Isto é feito através de um método simples SetLabelFontSize. Aqui é a classe final completo, que utiliza as resoluções dependentes da plataforma discutidos no Capítulo 5, &quot;Lidar com tamanhos&quot;:

public class AltLabel : Label

{

public static readonly BindableProperty PointSizeProperty = BindableProperty.Create(&quot;PointSize&quot;, // propertyName

typeof(double), // returnType typeof(AltLabel), // declaringType

8.0, // defaultValue

propertyChanged: OnPointSizeChanged);

public AltLabel()

{

SetLabelFontSize((double)PointSizeProperty.DefaultValue);

}

public double PointSize

{

set { SetValue(PointSizeProperty, value); }

get { return (double)GetValue(PointSizeProperty); }

}

static void OnPointSizeChanged(BindableObject bindable, object oldValue, object newValue)

{

((AltLabel)bindable).OnPointSizeChanged((double)oldValue, (double)newValue);

}

void OnPointSizeChanged(double oldValue, double newValue)

{

SetLabelFontSize(newValue);

}

void SetLabelFontSize(double pointSize)

{

FontSize = Device.OnPlatform(160, 160, 240) * pointSize / 72;

}

}

Também é possível para a propriedade instância OnPointSizeChanged para acessar a propriedade PointSize diretamente em vez de usar newValue. No momento em que o manipulador de propriedade alterada é chamado, o valor da propriedade subjacente já foi alterada. No entanto, você não tem acesso direto a esse valor subjacente, como você faz quando um campo privado apoia uma propriedade CLR. Esse valor subjacente é privado para BindableObject e só são acessíveis por meio da chamada GetValue.

Claro, nada impede que o código que está usando AltLabel de definir a propriedade FontSize e substituindo a configuração PointSize, mas vamos esperar que tal código é consciente disso. Aqui está algum código que é um programa chamado **PointSizedText** que usa AltLabel para exibir tamanhos em pontos de 4 a 12:

&lt;ContentPage xmlns=[&quot;http://xamarin.com/schemas/2014/form](http://xamarin.com/schemas/2014/forms)s&quot; xmlns:x=[&quot;http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)&quot; xmlns:toolkit=

&quot;clr-namespace:Xamarin.FormsBook.Toolkit;assembly=Xamarin.FormsBook.Toolkit&quot;

x:Class=&quot;PointSizedText.PointSizedTextPage&quot;&gt;

&lt;ContentPage.Padding&gt;

&lt;OnPlatform x:TypeArguments=&quot;Thickness&quot; iOS=&quot;5, 20, 0, 0&quot; Android=&quot;5, 0, 0, 0&quot; WinPhone=&quot;5, 0, 0, 0&quot; /&gt;

&lt;/ContentPage.Padding&gt;

&lt;StackLayout x:Name=&quot;stackLayout&quot;&gt;

&lt;toolkit:AltLabel Text=&quot;Text of 4 points&quot; PointSize=&quot;4&quot; /&gt;

&lt;toolkit:AltLabel Text=&quot;Text of 5 points&quot; PointSize=&quot;5&quot; /&gt;

&lt;toolkit:AltLabel Text=&quot;Text of 6 points&quot; PointSize=&quot;6&quot; /&gt;

&lt;toolkit:AltLabel Text=&quot;Text of 7 points&quot; PointSize=&quot;7&quot; /&gt;

&lt;toolkit:AltLabel Text=&quot;Text of 8 points&quot; PointSize=&quot;8&quot; /&gt;

&lt;toolkit:AltLabel Text=&quot;Text of 9 points&quot; PointSize=&quot;9&quot; /&gt;

&lt;toolkit:AltLabel Text=&quot;Text of 10 points&quot; PointSize=&quot;10&quot; /&gt;

&lt;toolkit:AltLabel Text=&quot;Text of 11 points&quot; PointSize=&quot;11&quot; /&gt;

&lt;toolkit:AltLabel Text=&quot;Text of 12 points&quot; PointSize=&quot;12&quot; /&gt;

&lt;/StackLayout&gt;

&lt;/ContentPage&gt;

E aqui estão os screenshots: