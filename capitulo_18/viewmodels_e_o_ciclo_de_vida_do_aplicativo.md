## ViewModels e o ciclo de vida do aplicativo {#viewmodels-e-o-ciclo-de-vida-do-aplicativo}

Em um programa de calculadora real em um dispositivo móvel, uma característica importante envolve a economia de todo o estado da calculadora quando o programa é encerrado, e restaurá-las quando o programa inicia-se novamente.

E mais uma vez, o conceito de ViewModel parece quebrar.

Claro, é possível escrever algum código de aplicativo que acessa as propriedades públicas do ViewModelo e salva-los, mas o estado da calculadora depende de campos privados também. O isSum- apresentado e campos accumulatedSum de AdderViewModel são essenciais para restaurar o estado do tor cálculo.

É óbvio que o código externo ao AdderViewModel não pode salvar e restaurar o estado do AdderViewModel sem o ViewModel expondo mais propriedades públicas. Há apenas uma classe que sabe o que é necessário para representar todo o estado interno de um ViewModel, e isso é o próprio ViewModel.

A solução é o ViewModel definir métodos públicos que salvar e restaurar seu estado interno. Mas porque um ViewModel deve se esforçar para ser independente de plataforma, estes métodos não devem usar alguma coisa específica para uma plataforma particular. Por exemplo, eles não devem acessar o objeto do aplciativo Xamarin.Forms, em seguida, adicionar itens a (ou recuperar itens de) Propriedades do dicionário desse objeto aplicação. Isso é demasiado específico para Xamarin.Forms.

No entanto, trabalhar com um objeto IDictionary genérica em métodos chamado SaveState e RestoreState é possível em qualquer ambiente .NET, e é assim que AdderViewModel implementa estes métodos:

public class AdderViewModel : ViewModelBase {

...

public void SaveState(IDictionary&lt;string, object&gt; dictionary)

{

dictionary[&quot;CurrentEntry&quot;] = CurrentEntry;

dictionary[&quot;HistoryString&quot;] = HistoryString;

dictionary[&quot;isSumDisplayed&quot;] = isSumDisplayed; dictionary[&quot;accumulatedSum&quot;] = accumulatedSum;

}

public void RestoreState(IDictionary&lt;string, object&gt; dictionary)

{

CurrentEntry = GetDictionaryEntry(dictionary, &quot;CurrentEntry&quot;, &quot;0&quot;);

HistoryString = GetDictionaryEntry(dictionary, &quot;HistoryString&quot;, &quot;&quot;);

isSumDisplayed = GetDictionaryEntry(dictionary, &quot;isSumDisplayed&quot;, false);

} 

O código na AddingMachine envolvido em salvar e restaurar este estado é principalmente implementado na classe App. A classe App instancia o AdderViewModel e chama RestoreState usando as propriedades dicionário da classe Application atual. Isso AdderViewModel é então passada como um argumento para o construtor AddingMachinePage:

public class App : Application

{

AdderViewModel adderViewModel;

public App()

{

adderViewModel = new AdderViewModel();

adderViewModel.RestoreState(Current.Properties);

MainPage = new AddingMachinePage(adderViewModel);

}

protected override void OnStart()

{

// Handle when your app starts.

}

protected override void OnSleep()

{

// Handle when your app sleeps.

adderViewModel.SaveState(Current.Properties);

}

protected override void OnResume()

{

// Handle when your app resumes.

}

}

A classe App também é responsável por chamar SaveState em AdderViewModel durante o processamento do método OnSleep.

O construtor AddingMachinePage apenas precisa de definir a instância de AdderViewModel ao BindingContext propriedade da página. O arquivo code-behind também gerencia a alternar entre layouts de retrato e paisagem:

public partial class AddingMachinePage : ContentPage {

public AddingMachinePage(AdderViewModel viewModel)

{

InitializeComponent();

// Set ViewModel as BindingContext.

}

{

BindingContext = viewModel;

void OnPageSizeChanged(object sender, EventArgs args)

// Portrait mode.

if (Width &lt; Height)

{

mainGrid.RowDefinitions[1].Height = GridLength.Auto;

mainGrid.ColumnDefinitions[1].Width = new GridLength(0, GridUnitType.Absolute);

Grid.SetRow(keypadGrid, 1);

Grid.SetColumn(keypadGrid, 0);

}

// Landscape mode.

else

{

mainGrid.RowDefinitions[1].Height = new GridLength(0, GridUnitType.Absolute);

mainGrid.ColumnDefinitions[1].Width = GridLength.Auto;

Grid.SetRow(keypadGrid, 0);

Grid.SetColumn(keypadGrid, 1);

}

}

}

O programa AddingMachine demonstra uma maneira de lidar com o ViewModel, mas não é o único caminho. Como alternativa, é possível para App para instanciar o AdderViewModel mas definem uma propriedade do tipo AdderViewModel que o construtor de AddingMachinePage pode acessar.

Ou, se você quiser que a página tem o controle total sobre o ViewModel, você pode fazer isso também. Adding- MachinePage pode definir seu próprio método OnSleep que é chamado a partir do método OnSleep na classe App, e a classe de página também pode lidar com a instanciação de AdderViewModel ea chamada dos métodos RestoreState e savestate. No entanto, esta abordagem pode tornar-se um pouco desajeitado para aplicações de várias páginas.

Em um aplicativo de várias páginas, você pode ter ViewModels separadas para cada página, talvez decorrente de um ViewModel com propriedades aplicáveis ​​a todo o aplicativo. Nesse caso, você vai querer evitar propriedades com o mesmo nome usando as mesmas chaves de dicionário para salvar o estado de cada ViewModel. Você pode usar mais extensas chaves de dicionário que incluem o nome da classe, por exemplo, &quot;Adder- ViewModel.CurrentEntry&quot;.

Embora o poder e as vantagens de ligação de dados e ViewModels deve ser aparente até agora, esses recursos realmente florescer quando usado com o ListView Xamarin.Forms. Isso é até no próximo capítulo.