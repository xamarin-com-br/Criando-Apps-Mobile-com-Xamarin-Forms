## Melhorando o ViewModel {#melhorando-o-viewmodel}

Uma implementação típica de INotifyPropertyChanged tem um campo de apoio privado para cada propriedade pública definida pela classe, por exemplo:

double number; 

Ele também tem um método PropertyChanged responsável por disparar o evento PropertyChanged:

protected void OnPropertyChanged(string propertyName) {

PropertyChangedEventHandler handler = PropertyChanged;

}

public double Number

{

if (handler != null)

{

}

PropertyChanged(this, new PropertyChangedEventArgs(propertyName));

A typical property definition looks like this:

set

{

if (number != value)

{

number = value;

OnPropertyChanged(&quot;Number&quot;);

// Do something with the new value.

Chapter 18 MVVM 514

}

}

get

{

}

return number;

}

Um problema potencial envolve a cadeia de texto que você passar para o método OnPropertyChanged. Se você cometer erros de ortografia, você não vai obter qualquer tipo de mensagem de erro, e ainda ligações que envolvem essa propriedade não funcionará. Além disso, o campo de backup aparece três vezes dentro desta propriedade única. Se você tivesse várias propriedades semelhantes e definiu-los através de operações de copiar e colar, é possível omitir a mudança de nome de uma das três aparições do campo de backup, e que o bug pode ser muito difícil de rastrear.

Você pode resolver o primeiro problema com um recurso introduzido no C # 5.0\. A classe tributo CallerMemberNameAt permite substituir um argumento método opcional com o nome do método de chamada ou propriedade.

Você pode fazer uso desse recurso, redefinindo o método OnPropertyChanged. Faça o argumento opcional, atribuindo nula a ele e precedendo-o com o atributo CallerMemberName entre colchetes. Você também vai precisar de uma diretiva utilizando para System.Runtime.CompilerServices:

protected void OnPropertyChanged([CallerMemberName] string propertyName = null) {

PropertyChangedEventHandler handler = PropertyChanged;

}

Agora a propriedade Number pode chamar o método OnPropertyChanged sem o argumento que indica o nome da propriedade. Esse argumento será definido automaticamente para o &quot;Número&quot; nome da propriedade, porque é onde a chamada para OnPropertyChanged é originário:

public double Number

{

if (handler != null)

{

PropertyChanged(this, new PropertyChangedEventArgs(propertyName));

}

set

{

if (number != value)

{

number = value;

OnPropertyChanged();

}

get

}

// Do something with the new value.

{

return number;

}

}

Essa abordagem evita um nome de propriedade de texto com erros ortográficos e também permite que nomes de propriedades a ser alterada durante o desenvolvimento do programa sem se preocupar com alterando também as cadeias de texto. De fato, uma das principais razões para que o atributo CallerMemberName foi inventado foi simplificar classes que implementam INotifyPropertyChanged.

No entanto, isso só funciona quando OnPropertyChanged é chamado a partir da propriedade cujo valor está mudando. No ColorViewModel anteriormente, nomes de propriedade explícita ainda seria necessário em todos, mas uma das chamadas para OnPropertyChanged.

É possível ir ainda mais longe para simplificar a lógica definida acessor: Você precisará definir um método genérico, provavelmente chamado SetProperty ou algo similar. Este método SetProperty também é definido com o atributo CallerMemberName:

bool SetProperty&lt;T&gt;(ref T storage, T value, [CallerMemberName] string propertyName = null) {

if (Object.Equals(storage, value))

return false;

storage = value;

OnPropertyChanged(propertyName);

return true;

}

protected void OnPropertyChanged([CallerMemberName] string propertyName = null) {

PropertyChangedEventHandler handler = PropertyChanged;

if (handler != null)

{

PropertyChanged(this, new PropertyChangedEventArgs(propertyName));

}

}

O primeiro argumento para SetProperty é uma referência para o campo de backup, e o segundo argumento é o valor a ser definido para a propriedade. SetProperty automatiza a verificação ea fixação do campo de backup. Observe que inclui explicitamente o argumento propertyName quando chamando OnProperty. (Caso contrário, o argumento propertyName se tornaria a string &quot;SetProperty&quot;!) O método retorna true se a propriedade foi alterada. Você pode usar esse valor de retorno para executar processamento adicional com o novo valor.

Embora SetProperty seja um método genérico, o compilador C # pode-se deduzir o tipo dos argumentos. Se você não precisa fazer nada com o novo valor no “accessor” conjunto de propriedades, você pode até mesmo reduzir os dois acessores para linhas simples sem obscurecer as operações:

public double Number

{

set { SetProperty(ref number, value); }

get { return number; }

}

Você pode gostar desta reformulação, tanto que você vai querer colocar o SetProperty e métodos OnPropertyChanged em sua própria classe e derivam dessa classe ao criar seu próprio ViewModels Tal classe, chamada ViewModelBase, já está na biblioteca Xamarin.FormsBook.Toolkit :

using System;

using System.ComponentModel;

using System.Runtime.CompilerServices;

namespace Xamarin.FormsBook.Toolkit

{

public class ViewModelBase : INotifyPropertyChanged

{

public event PropertyChangedEventHandler PropertyChanged;

protected bool SetProperty&lt;T&gt;(ref T storage, T value,

[CallerMemberName] string propertyName = null)

{

if (Object.Equals(storage, value))

return false;

storage = value;

OnPropertyChanged(propertyName);

return true;

}

protected void OnPropertyChanged([CallerMemberName] string propertyName = null)

{

PropertyChangedEventHandler handler = PropertyChanged;

if (handler != null)

}

{

}

PropertyChanged(this, new PropertyChangedEventArgs(propertyName));

}

}

}

}