## A Hierarquia da Classe Xamarin.Forms {#a-hierarquia-da-classe-xamarin-forms}

Antes de explorar os detalhes da importante classe bindableobject, vamos primeiro descobrir como a BindableObject se encaixa, em geral, na arquitetura Xamarin.Forms globais através da construção de uma hierarquia de classes.

Na estrutura da programação orientada a objetos como Xamarin.Forms, uma hierarquia de classes pode, muitas vezes, revelar importantes estruturas internas do ambiente. A hierarquia de classe revela como as várias classes relacionam-se com a outra, e as propriedades, métodos e eventos que partilham, incluindo como propriedades vinculáveis são suportados.

Você pode construir uma hierarquia de classe partindo laboriosamente através da documentação online e tomando nota de quais classes derivam de outras classes. Ou, você pode escrever um programa Xamarin.Forms para construir uma hierarquia de classes e exibi-lo no telefone. Tal programa faz uso de .NET reflexão para obter todas as classes públicas, estruturas e enumerações no Xamarin.Forms.Core, montando e organizando em uma árvore. A aplicação **ClassHierarchy** demonstra essa técnica.

Como de costume, o projeto **ClassHierarchy** contém uma classe que deriva de ContentPage chamado ClassHierarchyPage, mas também contém duas classes nomeada

O programa cria uma instância TypeInformation para cada classe pública (e estrutura e enumeração) na montagem Xamarin.Forms.Core, além de qualquer classe .NET, que serve como uma classe base para a classe Xamarin.Forms, com a exceção de objeto. (Essas classes .NET são Atributo, Delegado, Enum, EventArgs, Exception, MulticastDelegate, e ValueType.) O construtor de TypeInformation requer um Type objeto que identifica um tipo, mas também obtém algumas outras informações:

class TypeInformation

{

bool isBaseGenericType; Type baseGenericTypeDef;

public TypeInformation(Type type, bool isXamarinForms)

{

Type = type;

IsXamarinForms = isXamarinForms; TypeInfo typeInfo = type.GetTypeInfo(); BaseType = typeInfo.BaseType;

if (BaseType != null)

{

TypeInfo baseTypeInfo = BaseType.GetTypeInfo();

isBaseGenericType = baseTypeInfo.IsGenericType;

if (isBaseGenericType)

{

baseGenericTypeDef = baseTypeInfo.GetGenericTypeDefinition();

}

}

}

public Type Type { private set; get; }

public Type BaseType { private set; get; }

public bool IsXamarinForms { private set; get; }

public bool IsDerivedDirectlyFrom(Type parentType)

{

if (BaseType != null &amp;&amp; isBaseGenericType)

{

if (baseGenericTypeDef == parentType)

{

return true;

}

}

else if (BaseType == parentType)

{

return true;

}

return false;

}

}

Uma parte muito importante desta classe é o método IsDerivedDirectlyFrom, que retornará verdadeiro se passou um argumento que é o tipo base desse tipo. Esta determinação é complicada se classes genéricas estão envolvidas, e essa questão explica em grande parte da complexidade da classe.

A classe ClassAndSubclasses é consideravelmente mais curta:

class ClassAndSubclasses

{

public ClassAndSubclasses(Type parent, bool isXamarinForms)

{

Type = parent;

IsXamarinForms = isXamarinForms;

Subclasses = new List&lt;ClassAndSubclasses&gt;();

}

public Type Type { private set; get; }

public bool IsXamarinForms { private set; get; }

public List&lt;ClassAndSubclasses&gt; Subclasses { private set; get; }

}

O programa cria uma instância dessa classe para cada type exibido na hierarquia de classes, incluindo objeto, de modo que o programa cria mais de uma instância ClassAndSubclasses que o número de instâncias de TypeInformation. A instância ClassAndSubclasses associada com o objeto contém uma coleção de todas as classes que derivam diretamente do objeto, e cada uma dessas instâncias ClassAndSubclasses contém uma coleção de todas as classes que derivam daquela, e assim por diante, para o restante da hierarquia árvore.

A classe ClassHierarchyPage consiste em um arquivo XAML e um arquivo code-behind, mas o arquivo XAML contém pouco mais do que a rolagem do StackLayout pronto para alguns elementos do label:

&lt;ContentPage xmlns=[&quot;http://xamarin.com/schemas/2014/form](http://xamarin.com/schemas/2014/forms)s&quot; xmlns:x=[&quot;http://schemas.microsoft.com/winfx/2009/xaml](http://schemas.microsoft.com/winfx/2009/xaml)&quot; x:Class=&quot;ClassHierarchy.ClassHierarchyPage&quot;&gt;

&lt;ContentPage.Padding&gt;

&lt;OnPlatform x:TypeArguments=&quot;Thickness&quot; iOS=&quot;5, 20, 0, 0&quot; Android=&quot;5, 0, 0, 0&quot; WinPhone=&quot;5, 0, 0, 0&quot; /&gt;

&lt;/ContentPage.Padding&gt;

&lt;ScrollView&gt;

&lt;StackLayout x:Name=&quot;stackLayout&quot; Spacing=&quot;0&quot; /&gt;

&lt;/ScrollView&gt;

&lt;/ContentPage&gt;

O arquivo code-behind obtém referências para os dois objetos Xamarin.Forms Assembly e, em seguida .acumula todas as classes públicas, estruturas e enumerações na coleção classList. Em seguida, ele verifica a necessidade de incluir qualquer base de classes dos conjuntos .NET, classifica o resultado, e então chama dois métodos recorrentes, AddChildrenToParent e AddItemToStackLayout:

public partial class ClassHierarchyPage : ContentPage

{

public ClassHierarchyPage()

{

InitializeComponent();

List&lt;TypeInformation&gt; classList = new List&lt;TypeInformation&gt;();

// Get types in Xamarin.Forms.Core assembly. GetPublicTypes(typeof(View).GetTypeInfo().Assembly, classList);

// Get types in Xamarin.Forms.Xaml assembly. GetPublicTypes(typeof(Extensions).GetTypeInfo().Assembly, classList);

// Ensure that all classes have a base type in the list.

// (i.e., add Attribute, ValueType, Enum, EventArgs, etc.)

int index = 0;

// Watch out! Loops through expanding classList!

do

{

// Get a child type from the list. TypeInformation childType = classList[index];

if (childType.Type != typeof(Object))

{

bool hasBaseType = false;

// Loop through the list looking for a base type. foreach (TypeInformation parentType in classList)

{

if (childType.IsDerivedDirectlyFrom(parentType.Type))

{

hasBaseType = true;

}

}

// If there&#039;s no base type, add it.

if (!hasBaseType &amp;&amp; childType.BaseType != typeof(Object))

{

classList.Add(new TypeInformation(childType.BaseType, false));

}

}

index++;

}

while (index &lt; classList.Count);

// Now sort the list. classList.Sort((t1, t2) =&gt;

{

});return String.Compare(t1.Type.Name, t2.Type.Name)

// Start the display with System.Object.

ClassAndSubclasses rootClass = new ClassAndSubclasses(typeof(Object), false);

// Recursive method to build the hierarchy tree. AddChildrenToParent(rootClass, classList);

// Recursive method for adding items to StackLayout. AddItemToStackLayout(rootClass, 0);

}

void GetPublicTypes(Assembly assembly, List&lt;TypeInformation&gt; classList)

{

// Loop through all the types.

foreach (Type type in assembly.ExportedTypes)

{

TypeInfo typeInfo = type.GetTypeInfo();

// Public types only but exclude interfaces.

if (typeInfo.IsPublic &amp;&amp; !typeInfo.IsInterface)

{

// Add type to list.

classList.Add(new TypeInformation(type, true));

}

}

}

void AddChildrenToParent(ClassAndSubclasses parentClass, List&lt;TypeInformation&gt; classList)

{

foreach (TypeInformation typeInformation in classList)

{

if (typeInformation.IsDerivedDirectlyFrom(parentClass.Type))

{

ClassAndSubclasses subClass =

new ClassAndSubclasses(typeInformation.Type, typeInformation.IsXamarinForms);

parentClass.Subclasses.Add(subClass); AddChildrenToParent(subClass, classList);

}

}

}

void AddItemToStackLayout(ClassAndSubclasses parentClass, int level)

{

// If assembly is not Xamarin.Forms, display full name.

string name = parentClass.IsXamarinForms ? parentClass.Type.Name :

parentClass.Type.FullName; TypeInfo typeInfo = parentClass.Type.GetTypeInfo();

// If generic, display angle brackets and parameters. if (typeInfo.IsGenericType)

{

Type[] parameters = typeInfo.GenericTypeParameters;

name = name.Substring(0, name.Length - 2);

name += &quot;&lt;&quot;;

for (int i = 0; i &lt; parameters.Length; i++)

{

name += parameters[i].Name;

if (i &lt; parameters.Length - 1)

{

name += &quot;, &quot;;

}

}

name += &quot;&gt;&quot;;

}

// Create Label and add to StackLayout. Label label = new Label

{

Text = String.Format(&quot;{0}{1}&quot;, new string(&#039; &#039;, 4 * level), name), TextColor = parentClass.Type.GetTypeInfo().IsAbstract ?

Color.Accent : Color.Default

};

stackLayout.Children.Add(label);

// Now display nested types.

foreach (ClassAndSubclasses subclass in parentClass.Subclasses)

{

AddItemToStackLayout(subclass, level + 1);

}

}

}

O método AddChildrenToParent recursiva monta a lista ligada as instancias de ClassAndSubclasses da coleção classList plana.

O AddItemToStackLayout também é recursiva, pois é responsável por adicionar as ClassesAndSubclasses nas listas ligadss ao objeto StackLayout criando uma visualização da label para cada classe com um pouco de espaço em branco no início para o recuo adequado.

O método mostra os tipos Xamarin.Forms com apenas os nomes das classes, mas os tipos .NET com o nome totalmente qualificado para distingui-los. O método usa a cor de destaque plataforma para as classes que não são instanciáveis ou porque eles são abstratos ou estáticos:

No geral, você verá que elementos visuais das Xamarin.Forms têm a seguinte hierarquia geral:

System.Object

BindableObject

Element

VisualElement

View

...

Layout

...

Layout&lt;T&gt;

...

Page

...

Além do objeto, todas essas classes são implementadas na montagem Xamarin.Forms.Core.dll e associada a um espaço de nomes de Xamarin.Forms.

Vamos examinar algumas dessas classes principais em detalhes.

Como o nome da classe bindableobject já diz, a função primária desta classe é suportar a vinculação de dados, a ligação de duas propriedades, de dois objetos de modo que eles mantêm o mesmo valor. Mas bindableobject também suporta estilos e a extensão de marcação DynamicResource muito bem. Ele faz isso de duas maneiras diferentes: definições de propriedade bindableobject na forma de objetos BindableProperty, e também implementa a interface .NET INotifyPropertyChanged. Tudo isso será discutido com mais detalhes neste capítulo e nos capítulos futuros.

Vamos continuar a descer a hierarquia: Como você viu, objetos de interface do usuário em Xamarin.Forms são frequentemente organizados na página em uma hierarquia pai-filho, e a classe Element inclui suporte para pai e filho relacionamentos.

VisualElement é uma classe extremamente importante em Xamarin.Forms. Um elemento visual é qualquer coisa em Xamarin.Forms que ocupa uma área na tela. A classe VisualElement define 28 propriedades relacionadas a tamanho, localização, cor de fundo e outras características visuais e funcionais, tais como IsAtivado e IsVisable.

Em Xamarin.Forms a palavra view é muitas vezes usada para se referir a objetos visuais individuais, tais como botões, controles deslizantes e caixas de entrada de texto, mas você pode ver que a classe View é pai para as classes de layout também. Curiosamente, View só acrescenta um par de membros públicos do que ele herda da VisualElement. Aqueles incluem HorizontalOptions e VerticalOptions que fazem sentido porque essas propriedades não se aplicam a páginas - e GestureRecognizers para apoiar a entrada de toque.

Os descendentes de layout são capazes de ter views como filhas. Uma view filha aparece na tela visualmente dentro dos limites de seu pai. Classes que derivam de layout podem ter apenas uma filha do tipo View, mas a classe genérica Layout&lt;T&gt; define uma propriedade Children, que é uma coleção de várias views filhas, incluindo outros layouts. Você já viu que StackLayout, organiza seus filhos em uma pilha horizontal ou vertical. Embora a classe Layout derive de uma View, layouts são tão importantes em Xamarin.Forms que muitas vezes eles são considerados uma categoria em si mesmos.

**ClassHierarchy** lista todas as categorias de públicos, estruturas e enumerações definidas por Xamarin.Forms mas não lista interfaces. Aqueles são importantes também, mas você vai ter que explorá-los em seu próprio país. (Ou melhorar o programa para enumerá-los.)