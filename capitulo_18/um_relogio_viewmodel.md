## Um relógio ViewModel {#um-rel-gio-viewmodel}

Suponha que você está escrevendo um programa que precisa de acesso para a data e hora atual, e que você gostaria de usar essa informação através de ligações de dados. A biblioteca de classes base do .NET fornece data e hora informações através da estrutura DateTime. Para obter a data e hora atual, basta acessar a propriedade DateTime.Now. Essa é a forma habitual de escrever uma aplicação do relógio.

Mas para fins de ligação de dados, DateTime tem uma falha grave: Ele fornece apenas informações estáticas sem notificação quando a data ou a hora mudou.

No contexto da MVVM, a estrutura DateTime, talvez, qualifica-se como um modelo no sentido de que DateTime fornece todos os dados que precisamos, mas não de uma forma que seja propícia para a união de dados. É necessário escrever um ViewModel que faz uso de DateTime, mas fornece notificações quando a data ou hora mudou.

A biblioteca Xamarin.FormsBook.Toolkit contém a classe DateTimeViewModel mostrado abaixo. A classe tem apenas uma propriedade, que é nomeado DateTime do tipo DateTime, mas esta propriedade muda dinamicamente como resultado de chamadas frequentes para DateTime.Now em uma chamada de retorno Device.StartTimer.

Observe que a classe DateTimeViewModel é baseado na interface INotifyPropertyChanged e inclui uma diretiva using para o namespace System.ComponentModel que define esta interface. Para implementar essa interface, a classe define um evento público chamado PropertyChanged.

Cuidado com: É muito fácil definir um evento PropertyChanged em sua classe sem explicitamente especificar qual a classe que implementa INotifyPropertyChanged! As notificações serão ignorados se você não especificar explicitamente que a classe é baseada na interface INotifyPropertyChanged:

using System;

using System.ComponentModel;

using Xamarin.Forms;

public class DateTimeViewModel : INotifyPropertyChanged

Chapter 18 MVVM 495

DateTime dateTime = DateTime.Now;

}

{

public event PropertyChangedEventHandler PropertyChanged;

public DateTimeViewModel()

{

}

Device.StartTimer(TimeSpan.FromMilliseconds(15), OnTimerTick);

bool OnTimerTick()

{

DateTime = DateTime.Now;

return true;

}

public DateTime DateTime

{

private set

{

if (dateTime != value)

{

dateTime = value;

// Fire the event.

PropertyChangedEventHandler handler = PropertyChanged;

if (handler != null)

{

handler(this, new PropertyChangedEventArgs(&quot;DateTime&quot;));

}

}

}

get

}

{

}

return dateTime;

}

A única propriedade pública nesta classe é chamado de DateTime do tipo DateTime, e está associada com um campo de backup particular chamado dateTime. As propriedades públicas em ViewModels geralmente têm campos de apoio privados. O acessor set da propriedade DateTime é privado para a classe, e é atualizado a cada 15 milhões de milisegundos do retorno de chamada do timer.

Fora isso, o acessor conjunto é construído de uma forma muito padrão para ViewModels: ela primeiro verifica se o valor a ser definido para a propriedade é diferente do campo dateTime de apoio. Se não, ele define que o campo de backup a partir do valor de entrada e dispara o manipulador PropertyChanged com o nome da propriedade. Não é considerado boa prática disparar o manipulador PropertyChanged se a propriedade está apenas sendo definida para seu valor existente, e pode até levar a problemas envolvendo infinitos ciclos de configurações de propriedade recursiva em ligações nos dois sentidos.

Este é o código que aciona o evento:

PropertyChangedEventHandler handler = PropertyChanged;

if (handler != null)

{

handler(this, new PropertyChangedEventArgs(&quot;DateTime&quot;)); }

Essa forma é preferível a um código como esse, que não salva o manipulador em uma variável separada:

if (PropertyChanged != null)

{

PropertyChanged(this, new PropertyChangedEventArgs(&quot;DateTime&quot;)); }

Em um ambiente de vários segmentos, um manipulador de PropertyChanged pode ser destacado entre a instrução if que verifica se há um valor nulo e o disparo real do evento. Salvar o manipulador em uma variável impede causar um problema, por isso é um bom hábito de adotar, mesmo se você ainda não está trabalhando em um ambiente multithread.

O acessor get simplesmente retorna o campo de backup dateTime.

O programa MvvmClock demonstra como a classe DateTimeViewModel é capaz de fornecer atualizada data e hora para a interface do usuário através de ligações de dados:

&lt;ContentPage xmlns=&quot;http://xamarin.com/schemas/2014/forms&quot;

xmlns:x=&quot;http://schemas.microsoft.com/winfx/2009/xaml&quot;

xmlns:sys=&quot;clr-namespace:System;assembly=mscorlib&quot;

xmlns:toolkit=

&quot;clr-namespace:Xamarin.FormsBook.Toolkit;assembly=Xamarin.FormsBook.Toolkit&quot;

x:Class=&quot;MvvmClock.MvvmClockPage&quot;&gt;

&lt;ContentPage.Resources&gt;

&lt;ResourceDictionary&gt;

&lt;toolkit:DateTimeViewModel x:Key=&quot;dateTimeViewModel&quot; /&gt;

&lt;Style TargetType=&quot;Label&quot;&gt;

&lt;Setter Property=&quot;FontSize&quot; Value=&quot;Large&quot; /&gt;

&lt;Setter Property=&quot;HorizontalTextAlignment&quot; Value=&quot;Center&quot; /&gt;

&lt;/Style&gt;

&lt;/ResourceDictionary&gt;

&lt;/ContentPage.Resources&gt;

&lt;StackLayout VerticalOptions=&quot;Center&quot;&gt;

&lt;Label Text=&quot;{Binding Source={x:Static sys:DateTime.Now},

StringFormat=&#039;This program started at {0:F}&#039;}&quot; /&gt;

&lt;Label Text=&quot;But now...&quot; /&gt;

Chapter 18 MVVM 497

&lt;Label Text=&quot;{Binding Source={StaticResource dateTimeViewModel},

Path=DateTime.Hour,

StringFormat=&#039;The hour is {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Source={StaticResource dateTimeViewModel},

Path=DateTime.Minute,

StringFormat=&#039;The minute is {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Source={StaticResource dateTimeViewModel},

Path=DateTime.Second,

StringFormat=&#039;The seconds are {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Source={StaticResource dateTimeViewModel},

Path=DateTime.Millisecond,

&lt;/StackLayout&gt;

StringFormat=&#039;The milliseconds are {0}&#039;}&quot; /&gt;

&lt;/ContentPage&gt;

A seção Recursos para a página instancia o DateTimeViewModel e também define um estilo implícito para o Label.

O primeiro dos seis elementos do rótulo define sua propriedade de texto para um objeto Binding que envolve a estrutura DateTime .NET. A propriedade daquele que liga é: extensão de marcação estática que referencia a propriedade DateTime.Now estática para obter a data e hora em que o primeiro programa começa a funcionar. Nenhum caminho é necessário nesta ligação. O &quot;F&quot; especificação de formatação é para o padrão de data / tempo integral, com longas versões de data e hora cordas. Embora esta etiqueta exibe a data ea hora em que o programa é iniciado, ele nunca vai ficar atualizado.

As quatro ligações de dados finais serão atualizados. Nestas ligações de dados, a propriedade Source estiver definida como uma extensão de marcação StaticResource que referencia o objeto DateTimeViewModel. O caminho está definido para várias subpropriedades da propriedade DateTime de que ViewModel. Nos bastidores, a infra-estrutura de ligação atribui um manipulador sobre o evento PropertyChanged na DateTimeViewModel. Este manipulador verifica se há uma mudança na propriedade DateTime e atualiza a propriedade texto do rótulo, sempre que a propriedade mudanças.

O arquivo code-behind é vazio, exceto por uma chamada InitializeComponent.

As ligações de dados dos últimos quatro rótulos exibir um tempo atualizado que muda tão rápido quanto a taxa de atualização do vídeo:

A marcação neste arquivo XAML pode ser simplificado, definindo a propriedade BindingContext do StackLayout a uma extensão de marcação StaticResource que referencia o ViewModel. Isso é propagado através da árvore visual para que você possa remover as definições de Fonte sobre os quatro elementos finais do rótulo:

&lt;StackLayout VerticalOptions=&quot;Center&quot;

BindingContext=&quot;{StaticResource dateTimeViewModel}&quot;&gt;

&lt;Label Text=&quot;{Binding Source={x:Static sys:DateTime.Now},

&lt;Label Text=&quot;{Binding Path=DateTime.Hour,

StringFormat=&#039;This program started at {0:F}&#039;}&quot; /&gt;

StringFormat=&#039;The hour is {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Path=DateTime.Minute,

StringFormat=&#039;The minute is {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Path=DateTime.Second,

StringFormat=&#039;The seconds are {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Path=DateTime.Millisecond,

StringFormat=&#039;The milliseconds are {0}&#039;}&quot; /&gt;

&lt;/StackLayout&gt;

O binding na primeira etiqueta substitui o BindingContext com o seu próprio sistema de alimentação.

Você pode até mesmo remover o item DateTimeViewModel do ResourceDictionary e instanti- comeu bem no StackLayout entre BindingContext marcas de propriedade de elemento:

&lt;StackLayout VerticalOptions=&quot;Center&quot;&gt;

&lt;StackLayout.BindingContext&gt;

&lt;toolkit:DateTimeViewModel /&gt;

&lt;/StackLayout.BindingContext&gt;

&lt;Label Text=&quot;{Binding Source={x:Static sys:DateTime.Now},

StringFormat=&#039;This program started at {0:F}&#039;}&quot; /&gt;

&lt;Label Text=&quot;But now...&quot; /&gt;

&lt;Label Text=&quot;{Binding Path=DateTime.Hour,

StringFormat=&#039;The hour is {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Path=DateTime.Minute,

StringFormat=&#039;The minute is {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Path=DateTime.Second,

StringFormat=&#039;The seconds are {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Path=DateTime.Millisecond,

StringFormat=&#039;The milliseconds are {0}&#039;}&quot; /&gt;

&lt;/StackLayout&gt;

Ou, você pode definir a propriedade BindingContext do StackLayout para uma ligação que inclui a propriedade DateTime. O BindingContext torna-se então o valor DateTime, que permite as ligações individuais para simplesmente fazer referência propriedades da estrutura .NET DateTime

&lt;StackLayout VerticalOptions=&quot;Center&quot;

BindingContext=&quot;{Binding Source={StaticResource dateTimeViewModel},

Path=DateTime}&quot;&gt;

&lt;Label Text=&quot;{Binding Source={x:Static sys:DateTime.Now},

StringFormat=&#039;This program started at {0:F}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Path=Hour,

StringFormat=&#039;The hour is {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Path=Minute,

StringFormat=&#039;The minute is {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Path=Second,

StringFormat=&#039;The seconds are {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Path=Millisecond,

StringFormat=&#039;The milliseconds are {0}&#039;}&quot; /&gt;

&lt;/StackLayout&gt;

Você pode ter dúvidas de que isso vai funcionar! Nos bastidores, a ligação de dados instala um manipulador de eventos PropertyChanged e relógios para que propriedades particulares sejam alteradas, mas não pode, neste caso, porque a fonte de ligação de dados é um valor DateTime, e DateTime não implementa INotifyPropertyChanged . No entanto, o BindingContext desses elementos do rótulo muda com cada alteração para a propriedade DateTime no ViewModel, de modo a infra-estrutura de ligação acessa novos valores dessas propriedades.

Como as ligações individuais sobre as propriedades de texto diminuem em tamanho e complexidade, você pode retirar o caminho com o nome do atributo e colocar tudo em uma linha e ninguém vai ser confundida:

&lt;StackLayout VerticalOptions=&quot;Center&quot;&gt;

&lt;StackLayout.BindingContext&gt;

&lt;Binding Path=&quot;DateTime&quot;&gt;

&lt;Binding.Source&gt;

&lt;toolkit:DateTimeViewModel /&gt;

&lt;/Binding.Source&gt;

&lt;/Binding&gt;

&lt;/StackLayout.BindingContext&gt;

&lt;Label Text=&quot;{Binding Source={x:Static sys:DateTime.Now},

StringFormat=&#039;This program started at {0:F}&#039;}&quot; /&gt;

&lt;Label Text=&quot;But now...&quot; /&gt;

&lt;Label Text=&quot;{Binding Hour, StringFormat=&#039;The hour is {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Minute, StringFormat=&#039;The minute is {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Second, StringFormat=&#039;The seconds are {0}&#039;}&quot; /&gt;

&lt;Label Text=&quot;{Binding Millisecond, StringFormat=&#039;The milliseconds are {0}&#039;}&quot; /&gt;

&lt;/StackLayout&gt;

Em futuros programas neste livro, as ligações individuais a maioria irá ser tão curto e tão elegante quanto possível.