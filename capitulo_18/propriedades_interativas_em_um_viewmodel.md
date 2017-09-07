## Propriedades interativas em um ViewModel {#propriedades-interativas-em-um-viewmodel}

O segundo exemplo de um ViewModel faz algo tão básico que você nunca iria escrever uma ViewModel para esta finalidade. A classe SimpleMultiplierViewModel simplesmente multiplica dois números juntos. Mas é um bom exemplo para demonstrar a sobrecarga e mecânica de um ViewModel que tem várias propriedades interactivas. (E, embora você nunca escreveria um ViewModel para multiplicar dois números to- Gether, você pode escrever uma ViewModel para resolver equações de segundo grau ou algo muito mais complexo.)

The Simple classe Multiplicador ViewModel é parte do projeto simples multiplicador:

using System;

using System.ComponentModel;

class SimpleMultiplierViewModel : INotifyPropertyChanged

{

double multiplicand, multiplier, product;

public double Multiplicand

{

set

{

if (multiplicand != value)

{

multiplicand = value;

OnPropertyChanged(&quot;Multiplicand&quot;);

UpdateProduct();

}

}

get

{

return multiplicand;

}

}

public double Multiplier

{

set

{

if (multiplier != value)

{

multiplier = value;

OnPropertyChanged(&quot;Multiplier&quot;);

UpdateProduct();

}

}

get

{

return multiplier;

}

}

public double Product

{

protected set

{

if (product != value)

{

product = value;

OnPropertyChanged(&quot;Product&quot;);

}

}

get

{

return product;

}

}

void UpdateProduct()

{

Product = Multiplicand * Multiplier;

}

protected void OnPropertyChanged(string propertyName)

{

PropertyChangedEventHandler handler = PropertyChanged;

if (handler != null)

{

PropertyChanged(this, new PropertyChangedEventArgs(propertyName));

}

}

}

}

A classe define três propriedades públicas do tipo double, chamado Multiplicando, Multiplicador, e do produto. Cada propriedade é apoiado por um campo particular. O conjunto e obter assessores dos dois primeiros laços propriedades são públicos, mas o acessor definido da propriedade do produto é protegido para evitar que seja definida fora da classe, enquanto ainda permitindo uma classe descendente para mudá-lo.

O acessor conjunto de cada propriedade começa por verificar se o valor da propriedade é realmente tados, e se assim for, ele define o campo de apoio a esse valor e chama um método chamado OnPropertyChanged com esse nome propriedade.

A interface INotifyPropertyChanged não requer um método OnPropertyChanged, mas as classes ViewModel muitas vezes incluem um para reduzir a repetição de código. É geralmente definida como protegida em caso de necessidade para derivar um ViewModel do outro e disparar o evento na classe derivada. Mais adiante neste capítulo, você verá técnicas de reduzir a repetição de código nas classes INotifyPropertyChanged ainda mais.

Os “set accessors” para as propriedades multiplicando e multiplicador concluir chamando o método UpdateProduct. Este é o método que realiza o trabalho de multiplicar os valores das duas propriedades e estabelecendo um novo valor para a propriedade do produto, que, em seguida, dispara seu próprio evento Property-Changed.

Aqui está o arquivo XAML que faz uso deste ViewModel:

&lt;ContentPage xmlns=&quot;http://xamarin.com/schemas/2014/forms&quot;

xmlns:x=&quot;http://schemas.microsoft.com/winfx/2009/xaml&quot;

xmlns:local=&quot;clr-namespace:SimpleMultiplier&quot;

x:Class=&quot;SimpleMultiplier.SimpleMultiplierPage&quot;

Padding=&quot;10, 0&quot;&gt;

&lt;ContentPage.Resources&gt;

&lt;ResourceDictionary&gt;

&lt;local:SimpleMultiplierViewModel x:Key=&quot;viewModel&quot; /&gt;

&lt;Style TargetType=&quot;Label&quot;&gt;

&lt;Setter Property=&quot;FontSize&quot; Value=&quot;Large&quot; /&gt;

&lt;/Style&gt;

&lt;/ResourceDictionary&gt;

&lt;StackLayout BindingContext=&quot;{StaticResource viewModel}&quot;&gt;

&lt;StackLayout VerticalOptions=&quot;CenterAndExpand&quot;&gt;

&lt;Slider Value=&quot;{Binding Multiplicand}&quot; /&gt;

&lt;Slider Value=&quot;{Binding Multiplier}&quot; /&gt;

&lt;/StackLayout&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;

Spacing=&quot;0&quot;

VerticalOptions=&quot;CenterAndExpand&quot;

HorizontalOptions=&quot;Center&quot;&gt;

&lt;Label Text=&quot;{Binding Multiplicand, StringFormat=&#039;{0:F3}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Multiplier, StringFormat=&#039; x {0:F3}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Product, StringFormat=&#039; = {0:F3}&#039;}&quot; /&gt;

&lt;/StackLayout&gt;

&lt;/StackLayout&gt;

&lt;/ContentPage&gt;

O SimpleMultiplierViewModel é instanciado no dicionário Recursos e definido para a propriedade BindingContext do StackLayout usando uma extensão de marcação StaticResource. Isso BindingContext é herdada por todos os filhos e netos do StackLayout, que inclui duas Slider e três elementos do Rótulo. O uso do BindingContext permite que essas ligações para ser tão simples quanto possível.

O modo de ligação padrão da propriedade Value do Slider é TwoWay. Mudanças na propriedade Value de cada Slider causar alterações nas propriedades de ViewModel.

Os três elementos do rótulo exibir os valores de todas as três propriedades do ViewModel com alguma formatação que insere vezes e sinais de igual com os números:

&lt;Label Text=&quot;{Binding Multiplicand, StringFormat=&#039;{0:F3}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Multiplier, StringFormat=&#039; x {0:F3}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Product, StringFormat=&#039; = {0:F3}&#039;}&quot; /&gt;

Para os dois primeiros, você pode, alternativamente, vincular a propriedade Text dos Elementos do rótulo diretamente para a propriedade valor do controle deslizante correspondente, mas isso exigiria que você dá a cada Slider um nome com x: Nome e referência esse nome em um argumento de origem por usando o x: extensão de marcação de referência. A abordagem utilizada neste programa é muito mais limpo e verifica se os dados está a fazer uma viagem cheia através do ViewModel de cada Slider para cada etiqueta.

Não há nada no arquivo code-behind, exceto uma chamada para InitializeComponent no construtor. Toda a lógica de negócios está no ViewModel, e toda a interface de usuário é definida em XAML:

Se você gostaria, você pode inicializar o ViewModel como é instanciado no dicionário Recursos:

&lt;local:SimpleMultiplierViewModel x:Key=&quot;viewModel&quot;

Multiplicand=&quot;0.5&quot;

Multiplier=&quot;0.5&quot; /&gt;

Os elementos Slider vai ter esses valores iniciais como resultado da ligação a dois sentidos.

A vantagem de separar a interface do usuário da lógica de negócios subjacente torna-se evidente quando se deseja alterar a interface do usuário um pouco, talvez substituindo um Stepper para o Slider para um ou ambos os números:

&lt;StackLayout VerticalOptions=&quot;CenterAndExpand&quot;&gt;

&lt;Slider Value=&quot;{Binding Multiplicand}&quot; /&gt;

&lt;Stepper Value=&quot;{Binding Multiplier}&quot; /&gt;

Para além das diferentes gamas de os dois elementos, a funcionalidade é idêntica:

Você também pode substituir uma entrada:

&lt;StackLayout VerticalOptions=&quot;CenterAndExpand&quot;&gt;

&lt;Slider Value=&quot;{Binding Multiplicand}&quot; /&gt;

&lt;Entry Text=&quot;{Binding Multiplier}&quot; /&gt;

&lt;/StackLayout&gt;

O modo de ligação para a propriedade texto da entrada padrão também é TwoWay, então tudo que você precisa se preocupar é a conversão entre a propriedade de origem de casal e alvo de cadeia propriedade. Felizmente, esta conversão é manipulada pela infra-estrutura de ligação:

Se você digitar uma série de caracteres que não pode ser convertido para um casal, a ligação irá manter o último valor válido. Se você quiser validação mais sofisticado, você vai ter que implementar seu próprio (como com um gatilho, que será discutido no Capítulo 23).

Uma experiência interessante é digitar 1E-1, que é a notação científica que é conversível. Você vai vê-lo mudar imediatamente para &quot;0,1&quot; na entrada. Este é o efeito da TwoWay de ligação: A propriedade Multiplicadora está definida para 1E-1 a partir da entrada, mas o método ToString que a infra-estrutura de ligação chama quando o valor de volta para a entrada retorna o texto porque isso é diferente de &quot;0.1.&quot; e a partir do texto de entrada em vigor, o novo texto está definido. Para impedir que isso aconteça, você pode definir o modo de ligação a OneWayToSource:

&lt;StackLayout VerticalOptions=&quot;CenterAndExpand&quot;&gt;

&lt;Slider Value=&quot;{Binding Multiplicand}&quot; /&gt;

&lt;Entry Text=&quot;{Binding Multiplier, Mode=OneWayToSource}&quot; /&gt;

&lt;/StackLayout&gt;

Agora a propriedade Multiplicador do ViewModel é definido a partir da propriedade texto da entrada, mas não o contrário. Se você não precisar destes dois pontos de vista a ser atualizado a partir do ViewModel, você pode definir os dois para OneWayToSource. Mas geralmente você vai querer ligações MVVM ser TwoWay.

Se você se preocupar com ciclos infinitos em ligações nos dois sentidos? Normalmente não, porque propriedade- eventos alterados só são disparados quando a propriedade realmente muda e não quando é meramente definido para o mesmo valor. Geralmente a origem eo destino irá parar de atualizar-se após um salto ou dois. No entanto, é possível escrever um conversor de valor &quot;patológica&quot;, que não prevê de ida e volta as conversões, e que poderia realmente causar ciclos de atualização infinito em ligações nos dois sentidos.