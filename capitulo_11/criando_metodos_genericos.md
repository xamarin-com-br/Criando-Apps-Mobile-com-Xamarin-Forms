## Criando métodos genéricos {#criando-m-todos-gen-ricos}

Vários problemas podem surgir quando se define um objeto BindableProperty. Você pode cometer erros de ortografia do texto capitulação do nome da propriedade, ou você pode especificar um valor padrão que não é do mesmo tipo que a propriedade.

Pode eliminar estes dois problemas com uma forma genérica alternativa do método BindableProperty.Create. Os dois argumentos genéricos são o tipo da classe que define a propriedade e o tipo da própria propriedade. Com esta informação, o método pode derivar algumas das propriedades padrão e fornecer tipos de argumentos de valor padrão e os métodos de retorno de chamada. Além disso, o primeiro argumento para este método genérico alternativo deve ser um objeto LINQ Expressão referenciando a propriedade CLR. Isso permite que o método para obter a sequência de texto da propriedade.

A seguinte classe também AltLabelGeneric na biblioteca **Xamarin.FormsBook.Toolkit**, mas não fornecendo funcionalidade adicional sobre AltLabel -demonstra essa técnica, e, além disso usa uma função lambda para o retorno de chamada de propriedade alterado:

public class AltLabelGeneric : Label

{

public static readonly BindableProperty PointSizeProperty = BindableProperty.Create&lt;AltLabelGeneric, double&gt;

(label =&gt; label.PointSize,

8,

propertyChanged: (bindable, oldValue, newValue) =&gt;

{

});

((AltLabelGeneric)bindable).SetLabelFontSize(newValue

public AltLabelGeneric()

{

SetLabelFontSize((double)PointSizeProperty.DefaultValue);

}

public double PointSize

{

set { SetValue(PointSizeProperty, value); }

get { return (double)GetValue(PointSizeProperty); }

}

void SetLabelFontSize(double pointSize)

{

FontSize = Device.OnPlatform(160, 160, 240) * pointSize / 72;

}

}

O primeiro argumento para a forma genérica do método BindableProperty.Create é um objeto expressão referenciando a propriedade PointSize:

label =&gt; label.PointSize

O nome do objeto pode realmente ser muito curto:

l =&gt; l.PointSize

O método Create usa a reflexão sobre esse objeto Expressão para obter o nome do texto do CLR propriedade, que é &quot;PointSize&quot;.

Observe que o valor padrão é especificado como 8, em vez do 8.0\. Na versão genérica do método Binda- bleProperty.Create, este argumento é do mesmo tipo que o segundo argumento genérico, portanto, o simples 8 será convertido para um casal durante a compilação. Além disso, os argumentos OldValue e NewValue para o manipulador de propriedade alterado são do tipo casal e não tem que ser convertido.

Este método BindableProperty.Create genérico ajuda o seu código à prova de balas, mas não fornece nenhuma funcionalidade adicional. Interno a Xamarin.Forms, é convertido para o método BindableProperty.Create padrão.