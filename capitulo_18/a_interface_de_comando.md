## A Interface de Comando {#a-interface-de-comando}

Ligações de dados são muito poderosos.Ligações de dados conectam propriedades dos elementos visuais na Vista com propriedades de dados no ViewModel, e permitem que a manipulação direta de itens de dados através da interface do usuário.

Mas nem tudo é uma propriedade. Às vezes ViewModels expõem métodos públicos que devem ser chamados a partir do View com base na interação do usuário com um elemento visual. Sem MVVM, você provavelmente chamar esse método a partir de um manipulador de eventos Clicked de um botão ou um manipulador de eventos Tapped de um TapGestureRecognizer. Ao considerar essas necessidades, todo o conceito de ligações de dados e MVVM pode começar a parecer irremediavelmente falho. Como pode o arquivo code-behind de uma classe de página ser tirada de uma chamada InitializeComponent se ele ainda deve fazer chamadas de método do Vista para o ViewModel?

Não desista de MVVM tão rapidamente! Xamarin.Forms suporta um recurso que permite ligações de dados para fazer chamadas de método no ViewModel diretamente de Button e TapGestureRecognizer e alguns outros elementos. Este é um protocolo chamado a interface de comando ou a interface de comando.

A interface de comando é suportado por oito classes:

*   botão
*   MenuItem (abordados no Capítulo 19, &quot;vê a coleção&quot;), e, portanto, também ToolbarItem
*   Barra de pesquisa
*   TextCell, e, portanto, também ImageCell (também a ser abordados no Capítulo 19)
*   ListView (também a ser abordados no Capítulo 19)

TapGestureRecognizer Também é possível implementar comandando em suas próprias classes personalizadas. A interface de comando é provável que seja um pouco confuso no início. Vamos nos concentrar no Button. Botão define duas maneiras para o código para ser notificado quando o elemento é clicado. A primeira é o caso clicado. Mas você também pode usar a interface de comando do botão como uma alternativa ao (ou no ção adi- a) o evento clicado. Esta interface consiste em duas propriedades públicas que Button define:

Comando do tipo System.Windows.Input.ICommand.

CommandParameter do tipo Object. Para apoiar comandante, um ViewModel deve definir uma propriedade pública do tipo ICommand que é então ligado à propriedade de comando do botão através de dados normais de ligação.

Como INotifyPropertyChanged, a interface ICommand não é uma parte de Xamarin.Forms. É de- multado no namespace System.Windows.Input e implementado no System.ObjectModel assembléia, que é um dos conjuntos .NET ligados a uma aplicação Xamarin.Forms. ICommand é o único tipo no namespace System.Windows.Input que Xamarin.Forms suportes. De fato, é o único tipo em qualquer namespace System.Windows apoiado por Xamarin.Forms.

É uma coincidência que INotifyPropertyChanged e ICommand são ambos definidos no assembleias .NET em vez de Xamarin.Forms? Não. Estas interfaces são frequentemente utilizados em ViewModels, e alguns opers mento já pode ter ViewModels desenvolvido para um ou mais dos ambientes baseados em XAML da Microsoft. É mais fácil para os desenvolvedores a incorporar essas ViewModels existentes em Xamarin.Forms se INotifyPropertyChanged e ICommand são definidos em namespaces padrão NET e montagens ra- ther do que em Xamarin.Forms.

A interface ICommand define dois métodos e um evento:

public interface ICommand

{

vazio Executar (objeto arg);

    bool CanExecute (objeto arg);

evento EventHandler CanExecuteChanged;

}

O ViewModel define uma ou mais propriedades do tipo ICommand, o que significa que a propriedade é um tipo que implementa estes dois métodos e do evento. A propriedade em ViewModel que implementa ICommand pode então ser ligado à propriedade de comando de um botão. Quando o botão é clicado, o botão dispara seu evento normal clicados, como de costume, mas também chama o método de execução do objeto vinculado a sua propriedade Command. O argumento para o método de execução é o objeto definido para a propriedade CommandParameter do Button.

Essa é a técnica básica. No entanto, pode ser que determinadas condições no ViewModel proibir um clique de botão no momento atual. Nesse caso, o botão deve ser desativado. Este é o objectivo do método e do evento CanExecute CanExecuteChanged em ICommand. O botão chama CanExecute quando sua propriedade Command é primeiro set. Se CanExecute retorna false, o botão desativa próprio e não gera Executar chamadas. O botão também instala um manipulador para o evento CanExecuteChanged. Depois disso, sempre que o ViewModel aciona o evento CanExecuteChanged, o botão chama CanExecute novamente para determinar se o botão deve ser ativado.

A ViewModel que suporta a interface de comando define uma ou mais propriedades do tipo Command internamente define essa propriedade para uma classe que implementa a interface ICommand. O que é essa classe, e como ele funciona?

Se você estavam a implementando o protocolo dominante em um dos ambientes baseados em XAML da Microsoft, você estaria escrevendo sua própria classe que implementa ICommand, ou talvez usar um que você encontrou na web, ou um que foi incluído com algumas ferramentas MVVM. Às vezes, essas classes são nomeadas CommandDelegate ou algo similar.

Você pode usar essa mesma classe nos ViewModels de seus aplicativos Xamarin.Forms. No entanto, para sua conveniência, Xamarin.Forms inclui duas classes que implementam ICommand que você pode usar invés. Estas duas classes são chamados simplesmente de Comando e Comando &lt;T&gt;, onde T é o tipo dos argumentos para executar e CanExecute.

Se você está realmente compartilhando um ViewModel entre ambientes e Xamarin.Forms da Microsoft, você não pode usar as classes de comandos definidos pelo Xamarin.Forms. No entanto, você estará usando algo semelhante a estas classes de comando, de modo que a discussão seguinte será, certamente, aplicável independentemente.

A classe Command inclui os dois métodos e eventos da interface ICommand e também define um método ChangeCanExecute. Este método faz com que o objeto de comando para disparar o evento Changed CanExecute-, e essa facilidade acaba por ser muito útil.

Dentro do ViewModel, você provavelmente vai criar um objeto do tipo de comando ou Command &lt;T&gt; para cada propriedade pública no ViewModel do tipo ICommand. O Comando ou Command &lt;T&gt; é um método de retorno na forma de um objeto de Acção que é chamado quando o botão chama o método Execute da interface ICommand. O método CanExecute é opcional, mas assume a forma de um objeto Func que retorna booleano.

Em muitos casos, as propriedades do tipo ICommand são definidos no construtor do ViewModel e não mudar depois. Por essa razão, essas propriedades ICommand geralmente não precisa de fogo das Propriedades tyChanged eventos.

**Execuções simples de Métodos**

Vejamos um exemplo simples. Um programa chamado PowersOfThree permite usar dois botões para explorar várias potências de 3\. Um botão aumenta o expoente eo outro botão diminui o expoente.

A classe PowersViewModel deriva da classe ViewModelBase na biblioteca Xamarin.Forms- Book.Toolkit, mas o próprio ViewModel está no projeto de aplicativo PowersOfThree. Ele não se limita a potências de 3, mas o construtor requer um argumento de que a classe usa como um valor de base para o cálculo de energia, e que apresenta como a propriedade BaseValue. Como essa propriedade tem um acessor definido privado e não muda após o construtor conclui, a propriedade não dispara um evento PropertyChanged.

Para implementar a resposta aos toques Button, a classe PowersViewModel define duas propriedades do tipo ICommand, chamado IncreaseExponentCommand e DecreaseExponentCommand. Mais uma vez, ambas as propriedades de ter os acessores conjunto privadas. Como você pode ver, o construtor define essas duas propriedades de objetos de comando stantiating ções que fazem referência a métodos pouco privadas imediatamente a seguir ao tor construção. Estes dois métodos pequenos são chamados quando o método de execução de comando é chamado. O VierwModelo usa a classe de comando, em vez de Command &lt;T&gt; porque o programa não faz uso de qualquer argumento para os métodos execute:

class PowersViewModel : ViewModelBase

{

double exponent, power;

}

Exponent = 0;

// Initialize ICommand properties.

IncreaseExponentCommand = new Command(ExecuteIncreaseExponent); DecreaseExponentCommand = new Command(ExecuteDecreaseExponent);

}

void ExecuteIncreaseExponent()

{

Exponent += 1;

}

void ExecuteDecreaseExponent()

{

Exponent -= 1;

}

public PowersViewModel(double baseValue)

{

// Initialize properties.

BaseValue = baseValue;

public double BaseValue { private set; get; }

public double Exponent

{

private set

{

if (SetProperty(ref exponent, value))

{

Power = Math.Pow(BaseValue, exponent);

}

}

get

{

return exponent;

}

}

public double Power

{

private set { SetProperty(ref power, value); }

get { return power; }

}

public ICommand IncreaseExponentCommand { private set; get; } public ICommand DecreaseExponentCommand { private set; get; }

Os métodos ExecuteIncreaseExponent e ExecuteDecreaseExponent tanto mudam a propriedade Exponent (que dispara um evento PropertyChanged), ea propriedade Exponent recalcula a propriedade Power, que também aciona um evento PropertyChanged.

Muitas vezes um ViewModel irá instanciar seus objetos de comando, passando funções lambda para o construtor de comando. Essa abordagem permite que esses métodos para ser definido mesmo no structor con- ViewModel, assim:

IncreaseExponentCommand = new Command(() =&gt;

{

});

Exponent += 1;

DecreaseExponentCommand = new Command(() =&gt;

{

Exponent -= 1;

});

O arquivo PowersOfThreePage XAML vincula as propriedades de texto de três elementos label para o BaseValue, Exponent e propriedades poder da classe PowersViewModel, e liga-se as propriedades de comando dos dois elementos botão para as propriedades IncreaseExponentCommand e DecreaseExponentCommand do ViewModel.

Observe como um argumento de 3 é passado para o construtor de PowersViewModel como é instanciado no dicionário Resources. Passando argumentos para construtores ViewModel é a principal razão para a existência do x: tag Argumentos:

&lt;ContentPage xmlns=&quot;http://xamarin.com/schemas/2014/forms&quot;

xmlns:x=&quot;http://schemas.microsoft.com/winfx/2009/xaml&quot;

xmlns:local=&quot;clr-namespace:PowersOfThree&quot;

x:Class=&quot;PowersOfThree.PowersOfThreePage&quot;&gt;

&lt;ContentPage.Resources&gt;

&lt;ResourceDictionary&gt;

&lt;local:PowersViewModel x:Key=&quot;viewModel&quot;&gt;

&lt;x:Arguments&gt;

&lt;x:Double&gt;3&lt;/x:Double&gt;

&lt;/x:Arguments&gt;

&lt;/local:PowersViewModel&gt;

&lt;/ResourceDictionary&gt;

&lt;/ContentPage.Resources&gt;

&lt;StackLayout BindingContext=&quot;{StaticResource viewModel}&quot;&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;

Spacing=&quot;0&quot;

HorizontalOptions=&quot;Center&quot;

VerticalOptions=&quot;CenterAndExpand&quot;&gt;

&lt;Label FontSize=&quot;Large&quot;

Text=&quot;{Binding BaseValue, StringFormat=&#039;{0}&#039;}&quot; /&gt;

&lt;Label FontSize=&quot;Small&quot;

Text=&quot;{Binding Exponent, StringFormat=&#039;{0}&#039;}&quot; /&gt;

Chapter 18 MVVM 522

&lt;Label FontSize=&quot;Large&quot;

Text=&quot;{Binding Power, StringFormat=&#039; = {0}&#039;}&quot; /&gt;

&lt;/StackLayout&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;

VerticalOptions=&quot;CenterAndExpand&quot;&gt;

&lt;Button Text=&quot;Increase&quot;

Command=&quot;{Binding IncreaseExponentCommand}&quot;

HorizontalOptions=&quot;CenterAndExpand&quot; /&gt;

&lt;Button Text=&quot;Decrease&quot;

&lt;/StackLayout&gt;

Command=&quot;{Binding DecreaseExponentCommand}&quot;

HorizontalOptions=&quot;CenterAndExpand&quot; /&gt;

&lt;/StackLayout&gt;

&lt;/ContentPage&gt;

Sem tocar no ViewModel ou mesmo renomear um manipulador de eventos para que se aplique a uma torneira ra- ther de um botão, o programa funciona da mesma, mas com um olhar diferente: