## A infraestrutura Bindable {#a-infraestrutura-bindable}

Uma das mais básicas construções de linguagem C# é membro de uma classe conhecida como _a propriedade_. Todos nós, desde cedo, em nossos primeiros encontros com C# aprendemos a rotina geral da definição de uma propriedade. A propriedade é, muitas vezes, feita por um campo privado e inclui definir e obter acessores que referenciam o campo privado, fazendo algo com um novo valor:

public class MyClass

{

…

double quality;

public double Quality

{

set

{

}

get

{

}

}

…

}

quality = value;

// Do something with the new value

return quality;

As propriedades são, as vezes, referidas como &quot;campos inteligentes&quot;. Código que acessa uma propriedade sintaticamente assemelha-se ao código que acessa um campo. No entanto, a propriedade pode executar um pouco de seu próprio código quando a esta for acessada.

As propriedades são, também, como métodos. Na verdade, o código C# é compilado em Intermediate Language que implementa uma propriedade, como qualidade com um par de métodos chamados set_Quality e get_Quality. Contudo, apesar da semelhança funcional entre propriedades e o par de métodos definir e obter, a sintaxe da propriedade revela-se muito mais adequada quando movimentam-se do código para remarcar. É difícil imaginar XAML construído sobre uma API subjacente que está faltando propriedades.

Então, você pode se surpreender ao saber que Xamarin.Forms implementa uma definição de propriedade reforçada que se baseia em propriedades C#. Ou, talvez, você não será surpreendido. Se você já tem experiência com plataformas baseadas em XAML da Microsoft, você vai encontrar alguns conceitos familiares neste capítulo.

A definição de propriedade mostrado acima é conhecida como uma propriedade CLR porque é apoiado pelo .NET Common Language Runtime. A definição de propriedade reforçada em Xamarin.Forms se baseia no CLR propriedade e é chamado de propriedade vinculável encapsulado pela classe BindableProperty e apoiada pela classe bindableobject.