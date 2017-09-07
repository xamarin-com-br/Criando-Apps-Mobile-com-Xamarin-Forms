## Data binding e ViewModels {#data-binding-e-viewmodels}

Em muitas manifestações bastante simples de MVVM, o Modelo está ausente, ou apenas implícita, e o ViewModel contém toda a lógica de negócios. O View e ViewModel comunicam-se através de ligações de dados baseados em XAML. Os elementos visuais do View são alvos de ligação de dados, e propriedades no ViewModel e ligam as dados fontes.

Idealmente, um ViewModel deve ser independente de qualquer plataforma específica. Esta independência permite ViewModels para ser compartilhado entre outros ambientes baseados em XAML (como o Windows), além de Xamarin.Forms. Por esta razão, você deve tentar evitar o uso a seguinte declaração em seus modelos Ver:

using Xamarin.Forms;

Essa regra é freqüentemente quebrada neste capítulo! Um dos ViewModels baseia-se na estrutura de cores do Xamarin.Forms, e outra utiliza Device.StartTimer. Então, vamos chamar algo específico para Xamarin.Forms no ViewModel uma &quot;sugestão&quot; ao invés de uma &quot;regra&quot;.

elementos visuais do View qualificam como alvos de ligação de dados, porque as propriedades desses elementos visuais são apoiados por propriedades que podem ser ligadas. Para ser uma fonte de ligação de dados, um ViewModel deve iomplementar um protocolo de notificação para sinalizar quando uma propriedade em ViewModel mudou. Este protocolo de notificação é a interface INotifyPropertyChanged, que é definido no “namespace” System.Component-Model de forma muito simples, com apenas um evento:

public interface INotifyPropertyChanged{

event PropertyChangedEventHandler PropertyChanged; }

A interface INotifyPropertyChanged é tão central para mvvm que em discussões informais a interface é muitas vezes abreviada como INPC.

O evento PropertyChanged na interface INotifyPropertyChanged é do tipo Property-Changed-EventHandler. Um manipulador para este manipulador de eventos PropertyChanged recebe uma instância da classe PropertyChangedEventArgs, que define uma única propriedade chamada PropertyName do tipo string indicando o que propriedade em ViewModel mudou. O manipulador de eventos pode acessar essa propriedade.

Uma classe que implementa INotifyPropertyChanged deve disparar um evento PropertyChanged sempre que uma propriedade for alterada, mas a classe não deve acionar o evento quando a propriedade é meramente definido, mas não mudou.

Algumas classes definem propriedades de propriedades que são inicializados no construtor e são imutáveis. Essas propriedades não precisa acionar eventos PropertyChanged porque um manipulador dades ertyChanged pode ser ligado somente após o código no construtor termina, e estas propriedades nunca mudam após esse tempo.

Em teoria, uma classe ViewModel pode ser derivada de bindableobject e implementar as suas propriedades públicas como objetos BindableProperty. Bindableobject implementa INotifyPropertyChanged e automaticamente dispara um evento PropertyChanged quando qualquer propriedade apoiado por uma BindableProperty mudanças. Mas decorrente bindableobject é um exagero para um ViewModel. Porque bindableobject e BindableProperty são específicos para Xamarin.Forms, tal ViewModel já não é plataforma Independentes, ea técnica não oferece vantagens reais sobre uma implementação mais simples de INotify- PropertyChanged.