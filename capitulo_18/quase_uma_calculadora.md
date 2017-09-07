## Quase uma calculadora {#quase-uma-calculadora}

Agora é hora de fazer um ViewModel mais sofisticado com objetos ICommand. O próximo programa é quase como uma calculadora, exceto que ele só acrescenta uma série de números juntos. O ViewModel é nomeado AdderViewModel, e o programa é chamado de Máquina de adição. Vamos olhar para os screenshots primeiros:

Na parte superior da página você pode ver uma história da série de números que já foram introduzi- e adicionados. Esta é uma etiqueta em uma ScrollView, por isso pode ficar um pouco longo.

A soma desses números é exibido na visualização de entrada acima do teclado. Normalmente, esse ponto de vista Entrada contém o número que você está digitando, mas depois de bater o grande sinal de mais à direita do teclado, a entrada exibe a soma acumulada e o botão de sinal fica desativado. Você precisa começar a digitar outro número para o valor acumulado a desaparecer e para o botão com o sinal de mais para ser ativado. Da mesma forma, o botão de retrocesso é ativado assim que você começa a digitar.

Estas não são as únicas teclas que podem ser desativados. O ponto decimal é desativado quando o número que já está digitando tem um ponto decimal, e todas as teclas numéricas tornar-se deficientes quando o número contém 16 caracteres. Isso é para evitar o número na entrada de se tornar demasiado longo para exibir.

A desativação destes botões é o resultado da implementação do método CanExecute na interface ICom- mand.

A classe AdderViewModel está na biblioteca Xamarin.FormsBook.Toolkit e deriva Ver- ModelBase. Aqui está a parte da classe com todas as propriedades públicas e suas áreas de apoio:

public class AdderViewModel : ViewModelBase {

string currentEntry = &quot;0&quot;;

string historyString = &quot;&quot;;

Chapter 18 MVVM

525

...

public string CurrentEntry

{

}

private set { SetProperty(ref currentEntry, value); }

get { return currentEntry; }

public string HistoryString

{

private set { SetProperty(ref historyString, value); }

}

public ICommand

public ICommand

public ICommand

public ICommand

public ICommand

ClearCommand { private set; get; } ClearEntryCommand { private set; get; } BackspaceCommand { private set; get; } NumericCommand { private set; get; } DecimalPointCommand { private set; get; }

...

get { return historyString; }

public ICommand AddCommand { private set; get; }

}

Todas as propriedades de ter os acessores conjunto privadas. As duas propriedades do tipo string são apenas definir internamente com base nas torneiras-chave e as propriedades do tipo ICommand são definidos no structor con- AdderViewModel (que você verá em breve).

Estes oito propriedades públicas são a única parte do AdderViewModel que o arquivo XAML no projeto AddingMachine precisa saber sobre. Aqui está o arquivo XAML. Ele contém uma grade principal de dois linha e coluna dois para alternar entre modo retrato e paisagem, e uma etiqueta, de entrada, e 15 elementos botão, todos os quais estão vinculados a uma das oito propriedades públicas do AdderViewModel. Observe que as propriedades de comando de todas as 10 teclas numéricas são obrigados a propriedade NumericCommand e que os botões são diferenciados pela propriedade CommandParameter. A definição dessa propriedade CommandParameter é passado como um argumento para os métodos Execute e CanExecute:

&lt;ContentPage xmlns=&quot;http://xamarin.com/schemas/2014/forms&quot;

xmlns:x=&quot;http://schemas.microsoft.com/winfx/2009/xaml&quot;

x:Class=&quot;AddingMachine.AddingMachinePage&quot;

SizeChanged=&quot;OnPageSizeChanged&quot;&gt;

&lt;ContentPage.Padding&gt;

&lt;OnPlatform x:TypeArguments=&quot;Thickness&quot;

iOS=&quot;10, 20, 10, 10&quot;

Android=&quot;10&quot;

WinPhone=&quot;10&quot; /&gt;

&lt;/ContentPage.Padding&gt;

Chapter 18 MVVM 526

&lt;Grid x:Name=&quot;mainGrid&quot;&gt;

&lt;!-- Initialized for Portrait mode. --&gt;

&lt;Grid.RowDefinitions&gt;

&lt;RowDefinition Height=&quot;*&quot; /&gt;

&lt;RowDefinition Height=&quot;Auto&quot; /&gt;

&lt;/Grid.RowDefinitions&gt;

&lt;Grid.ColumnDefinitions&gt;

&lt;ColumnDefinition Width=&quot;*&quot; /&gt;

&lt;ColumnDefinition Width=&quot;0&quot; /&gt;

&lt;/Grid.ColumnDefinitions&gt;

&lt;!-- History display. --&gt;

&lt;ScrollView Grid.Row=&quot;0&quot; Grid.Column=&quot;0&quot;

&lt;!-- Keypad. --&gt;

Padding=&quot;5, 0&quot;&gt;

&lt;Label Text=&quot;{Binding HistoryString}&quot; /&gt;

&lt;/ScrollView&gt;

&lt;Grid x:Name=&quot;keypadGrid&quot;

Grid.Row=&quot;1&quot; Grid.Column=&quot;0&quot;

RowSpacing=&quot;2&quot;

ColumnSpacing=&quot;2&quot;

WidthRequest=&quot;240&quot;

HeightRequest=&quot;360&quot;

VerticalOptions=&quot;Center&quot;

HorizontalOptions=&quot;Center&quot;&gt;

&lt;Grid.Resources&gt;

&lt;ResourceDictionary&gt;

&lt;Style TargetType=&quot;Button&quot;&gt;

&lt;Setter Property=&quot;FontSize&quot; Value=&quot;Large&quot; /&gt;

&lt;Setter Property=&quot;BorderWidth&quot; Value=&quot;1&quot; /&gt;

&lt;/Style&gt;

&lt;/ResourceDictionary&gt;

&lt;/Grid.Resources&gt;

&lt;Label Text=&quot;{Binding CurrentEntry}&quot;

Grid.Row=&quot;0&quot; Grid.Column=&quot;0&quot; Grid.ColumnSpan=&quot;4&quot;

FontSize=&quot;Large&quot;

LineBreakMode=&quot;HeadTruncation&quot;

VerticalOptions=&quot;Center&quot;

HorizontalTextAlignment=&quot;End&quot; /&gt;

&lt;Button Text=&quot;C&quot;

Grid.Row=&quot;1&quot; Grid.Column=&quot;0&quot;

Command=&quot;{Binding ClearCommand}&quot; /&gt;

&lt;Button Text=&quot;CE&quot;

Grid.Row=&quot;1&quot; Grid.Column=&quot;1&quot;

Command=&quot;{Binding ClearEntryCommand}&quot; /&gt;

&lt;Button Text=&quot;&amp;#x21E6;&quot;

Grid.Row=&quot;1&quot; Grid.Column=&quot;2&quot;

Command=&quot;{Binding BackspaceCommand}&quot; /&gt;

&lt;Button Text=&quot;+&quot;

&lt;Button Text=&quot;7&quot;

Grid.Row=&quot;1&quot; Grid.Column=&quot;3&quot; Grid.RowSpan=&quot;5&quot;

Command=&quot;{Binding AddCommand}&quot; /&gt;

Grid.Row=&quot;2&quot; Grid.Column=&quot;0&quot;

Command=&quot;{Binding NumericCommand}&quot;

CommandParameter=&quot;7&quot; /&gt;

&lt;Button Text=&quot;8&quot;

Grid.Row=&quot;2&quot; Grid.Column=&quot;1&quot;

Command=&quot;{Binding NumericCommand}&quot;

CommandParameter=&quot;8&quot; /&gt;

&lt;Button Text=&quot;9&quot;

Grid.Row=&quot;2&quot; Grid.Column=&quot;2&quot;

Command=&quot;{Binding NumericCommand}&quot;

CommandParameter=&quot;9&quot; /&gt;

&lt;Button Text=&quot;4&quot;

Grid.Row=&quot;3&quot; Grid.Column=&quot;0&quot;

&lt;Button Text=&quot;5&quot;

Command=&quot;{Binding NumericCommand}&quot;

CommandParameter=&quot;4&quot; /&gt;

&lt;Button Text=&quot;6&quot;

Grid.Row=&quot;3&quot; Grid.Column=&quot;1&quot;

Command=&quot;{Binding NumericCommand}&quot;

CommandParameter=&quot;5&quot; /&gt;

Grid.Row=&quot;3&quot; Grid.Column=&quot;2&quot;

Command=&quot;{Binding NumericCommand}&quot;

CommandParameter=&quot;6&quot; /&gt;

&lt;Button Text=&quot;1&quot;

Grid.Row=&quot;4&quot; Grid.Column=&quot;0&quot;

&lt;Button Text=&quot;2&quot;

Command=&quot;{Binding NumericCommand}&quot;

CommandParameter=&quot;1&quot; /&gt;

&lt;Button Text=&quot;3&quot;

Grid.Row=&quot;4&quot; Grid.Column=&quot;1&quot;

Command=&quot;{Binding NumericCommand}&quot;

CommandParameter=&quot;2&quot; /&gt;

Grid.Row=&quot;4&quot; Grid.Column=&quot;2&quot;

Command=&quot;{Binding NumericCommand}&quot;

CommandParameter=&quot;3&quot; /&gt;

&lt;Button Text=&quot;0&quot;

Grid.Row=&quot;5&quot; Grid.Column=&quot;0&quot; Grid.ColumnSpan=&quot;2&quot;

Command=&quot;{Binding NumericCommand}&quot;

CommandParameter=&quot;0&quot; /&gt;

&lt;/Grid&gt;

&lt;Button Text=&quot;&amp;#x00B7;&quot;

&lt;/Grid&gt;

Grid.Row=&quot;5&quot; Grid.Column=&quot;2&quot;

Command=&quot;{Binding DecimalPointCommand}&quot; /&gt;

&lt;/ContentPage&gt;

O que você não vai encontrar no arquivo XAML é uma referência a AdderViewModel. Por razões que você verá em breve, AdderViewModel é instanciado no código.

O núcleo da lógica do programa está em execução e os métodos CanExecute das seis propriedades mand ICom-. Estas propriedades são todos inicializado no construtor AdderViewModel mostrado abaixo, e a Execute e métodos CanExecute são todas as funções lambda.

Quando apenas uma função lambda aparece no construtor de comando, que é o método de execução (como o nome do parâmetro indica) e o botão está sempre ativada. Este é o caso de ClearCom- mand e ClearEntryCommand.

Todos os outros construtores de comando tem duas funções lambda. O primeiro é o método de execução, eo segundo é o método CanExecute. O método CanExecute retorna true se o botão deve ser ativado e false de outra forma.

Todas as propriedades ICommand são definidos com a forma nongeneric da classe Command exceto para nu- mericCommand, o que requer um argumento para os métodos Execute e CanExecute para identificar qual chave tem sido aproveitado:

public class AdderViewModel : ViewModelBase {

...

bool isSumDisplayed = false;

double accumulatedSum = 0;

public AdderViewModel()

{

ClearCommand = new Command(

execute: () =&gt;

{

HistoryString = &quot;&quot;;

accumulatedSum = 0;

CurrentEntry = &quot;0&quot;;

isSumDisplayed = false;

RefreshCanExecutes();

});

ClearEntryCommand = new Command(

execute: () =&gt;

{

CurrentEntry = &quot;0&quot;;

isSumDisplayed = false;

RefreshCanExecutes();

});

Chapter 18 MVVM 529

BackspaceCommand = new Command(

execute: () =&gt;

{

CurrentEntry = CurrentEntry.Substring(0, CurrentEntry.Length - 1);

if (CurrentEntry.Length == 0)

{

CurrentEntry = &quot;0&quot;;

}

},

RefreshCanExecutes();

canExecute: () =&gt;

{

});

return !isSumDisplayed &amp;&amp; (CurrentEntry.Length &gt; 1 || CurrentEntry[0] != &#039;0&#039;);

NumericCommand = new Command&lt;string&gt;(

execute: (string parameter) =&gt;

{

if (isSumDisplayed || CurrentEntry == &quot;0&quot;)

else

CurrentEntry = parameter;

CurrentEntry += parameter;

isSumDisplayed = false;

},

RefreshCanExecutes();

canExecute: (string parameter) =&gt;

{

return isSumDisplayed || CurrentEntry.Length &lt; 16;

});

DecimalPointCommand = new Command(

execute: () =&gt;

{

if (isSumDisplayed)

CurrentEntry = &quot;0.&quot;;

else

CurrentEntry += &quot;.&quot;;

isSumDisplayed = false;

RefreshCanExecutes();

},

canExecute: () =&gt;

{

});

return isSumDisplayed || !CurrentEntry.Contains(&quot;.&quot;);

AddCommand = new Command(

execute: () =&gt;

{

double value = Double.Parse(CurrentEntry);

Chapter 18 MVVM 530

HistoryString += value.ToString() + &quot; + &quot;;

accumulatedSum += value;

CurrentEntry = accumulatedSum.ToString();

isSumDisplayed = true;

RefreshCanExecutes();

},

canExecute: () =&gt;

{

});

return !isSumDisplayed;

}

void RefreshCanExecutes()

{

((Command)BackspaceCommand).ChangeCanExecute();

((Command)NumericCommand).ChangeCanExecute();

((Command)DecimalPointCommand).ChangeCanExecute();

((Command)AddCommand).ChangeCanExecute();

} ...

}

Todos os métodos Executar concluir chamando um método chamado RefreshCanExecute seguindo o construtor. Este método chama o método ChangeCanExecute de cada um dos quatro objetos Comando que implementam métodos CanExecute. Essa chamada de método faz com que o objeto de comando para disparar um evento ChangeCanExecute. Cada botão responde a esse evento, fazendo outra chamada para o método CanExecute para determinar se o botão deve ser habilitado ou não.

Não é necessário que cada método para concluir com um apelo a todos os quatro métodos ChangeCanExecute. Por exemplo, o método para a ChangeCanExecute DecimalPointCommand não necessita de ser chamado quando o método de execução para NumericCommand executa. No entanto, acabou por ser mais fácil, tanto em termos de lógica e código de consolidação-a simplesmente chamá-los todos após cada toque chave.

Você pode ser mais confortável implementação destas Execute e métodos CanExecute como métodos não regulares, em vez de funções lambda. Ou você pode ser mais confortável ter apenas um objeto dade mand que lida com todas as chaves. Cada tecla pode ter uma cadeia CommandParameter identificação e você pode distinguir entre eles com um switch e caso.

Há muitas maneiras de implementar a lógica de comando, mas deve ficar claro que o uso de comandar tende a estruturar o código de uma maneira flexível e ideal.

Uma vez que a lógica adicionando está no lugar, por que não acrescentar um par de mais botões para a subtração, a multiplicação e divisão?

Bem, não é tão fácil de melhorar a lógica de aceitar várias operações ao invés de apenas uma operação. Se o programa suporta múltiplas operações, em seguida, quando o usuário digita uma das teclas de operação, esta operação tem de ser guardado para aguardar o próximo número. Somente após o próximo número é completado (sinalizado pela imprensa de uma outra tecla de operação ou igual a chave) é que salvou operação aplicado.

Uma abordagem mais fácil seria escrever uma calculadora notação polonesa reversa (RPN), onde a operação segue a entrada do segundo número. A simplicidade da lógica RPN é uma grande razão pela qual culators RPN cal- apelar para programadores tanto!