## Propriedades e atributos {#propriedades-e-atributos}

Aqui está uma Label Xamarin.Forms instanciada e inicializada em código, tanto quanto poderia parecer no construtor de uma classe de página:

new Label

{

Text = &quot;Hello from Code!&quot;, IsVisible = true,

Opacity = 0.75,

XAlign = TextAlignment.Center,

VerticalOptions = LayoutOptions.CenterAndExpand, TextColor = Color.Blue,

BackgroundColor = Color.FromRgb(255, 128, 128),

FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), FontAttributes = FontAttributes.Bold | FontAttributes.Italic

};

Aqui está uma Label muito semelhante instanciada e inicializada em XAML, que você pode ver imediatamente é mais conciso do que o código equivalente:

&lt;Label Text=&quot;Hello from XAML!&quot; IsVisible=&quot;True&quot; Opacity=&quot;0.75&quot;

XAlign=&quot;Center&quot;

VerticalOptions=&quot;CenterAndExpand&quot;

TextColor=&quot;Blue&quot;

BackgroundColor=&quot;#FF8080&quot;

FontSize=&quot;Large&quot;

FontAttributes=&quot;Bold,Italic&quot; /&gt;

Classe em Xamarin.Forms como Label se tornar elementos XML em XAML. Propriedades como Text, IsVisible, e o resto se tornar atributos XML em XAML.

Para ser instanciada em XAML, uma classe como a Label deve ter um construtor sem parâmetros públicos. (No próximo capítulo, você vai ver que há uma técnica para passar argumentos para um construtor em XAML, mas é geralmente utilizado para fins especiais). As propriedades definidas em XAML deve ter assessores set públicas. Por convenção, os espaços cercam um sinal de igual no código, mas não em XML (ou XAML), mas você pode usar tanto espaço em branco como você quer.

A concisão do XAML resulta principalmente da brevidade do exemplo de atributo valores for, o uso da palavra &quot;Grande&quot; em vez de uma chamada para o método Device.GetNamedSize. Essas abreviaturas não são incorporadas ao analisador XAML. O analisador XAML em vez disso é assistida por várias classes de conversor definidas especificamente para esta finalidade.

Quando o analisador XAML encontra o elemento Label, ele pode usar o reflexo para determinar se Xamarin.Forms tem uma Label classe chamada e, nesse caso, é possível criar uma instância dessa classe. Agora ele está pronto para inicializar esse objeto. A propriedade de Text é do tipo string, eo valor do atributo é simplesmente atribuído a essa propriedade.

Porque XAML é XML, você pode incluir caracteres Unicode no texto usando a sintaxe XML padrão. Preceder o valor decimal Unicode com &amp;# (ou o valor hexadecimal Unicode com &amp;#x) e segui-lo com um ponto e vírgula:

Text=&quot;Cost &amp;#x2014; &amp;#x20AC;123.45&quot;

Esses são os valores Unicode para o travessão e símbolo do euro. Para forçar uma quebra de linha, use o caractere de avanço de linha &amp;#x000A, ou (porque zeros à esquerda não são obrigados) &amp;#xA, ou com o código decimal, &amp;#10.

As aspas têm um significado especial em XML, de modo a incluir esses caracteres em uma sequência de texto, use uma das entidades predefinidas padrão:

*   &amp;lt; for &lt;
*   &amp;gt; for &gt;
*   &amp;amp; for &amp;
*   &amp;apos; for &#039;
*   &amp;quot; for &quot;

As entidades predefinidas HTML como &amp;nbsp; não são suportados. Para um uso do espaço não separável &amp;#xA0; em vez disso.

Além disso, chaves ({ and }) tem um significado especial em XAML. Se você precisa para começar um valor de atributo com uma chave esquerda, comece com um par de chaves ({}) e, em seguida, a chave esquerda.

As propriedades IsVisible e Opacity da Label são do tipo bool e double, respectivamente, e estes são tão simples como se poderia esperar. O analisador XAML utiliza os métodos Boolean.Parse e Double.Parse para converter os valores de atributos. O método Boolean.Parse é caso insensível, mas os valores booleanos geralmente são capitalizados como &quot;True&quot; e &quot;False&quot; em XAML. O método Double.Parse é passado um argumento CultureInfo.InvariantCulture, de modo que a conversão não depende da cultura local do programador ou utilizador.

A propriedade XAlign da Label é do tipo TextAlignment, que é uma enumeração. Para qualquer propriedade que é um tipo de enumeração, o analisador XAML usa o método Enum.Parse para converter a partir da cadeia de valor.

A propriedade VerticalOptions é do tipo LayoutOptions, uma estrutura. Quando o analisador XAML referência a estrutura LayoutOptions usando a reflexão, descobre que a estrutura tem um atributo de C# definida:

[TypeConverter (typeof(LayoutOptionsConverter))]

public struct LayoutOptions

{

…

}

(Atenção! Essa discussão envolve dois tipos de atributos: Atributos XML como XAlign e C# atributos como este TypeConverter)

O atributo TypeConverter é apoiada por uma classe chamada TypeConverterAttribute. Este atributo TypeConverter em LayoutOptions faz referência a uma classe chamada LayoutOptionsConverter. Esta é uma classe privada para Xamarin.Forms, mas deriva de uma classe abstrata chamada pública TypeConverter que define métodos chamados CanConvertFrom e ConvertFrom. Quando o analisador XAML encontra esse atributo TypeConverter, ele instancia o LayoutOptionsConverter. Os VerticalOptions atribuir no XAML é atribuído a cadeia &quot;Center&quot;, de modo que o analisador XAML passa que &quot;Center&quot; string para o método ConvertFrom de LayoutOptionsConverter, e fora aparece um valor LayoutOptions. Isto é atribuído à propriedade VerticalOptions do objecto da Label.

Da mesma forma, quando o analisador XAML encontra as propriedades TextColor e BackgroundColor, ele usa reflexão para determinar que essas propriedades são do tipo Color. A estrutura de cores também é adornada com um atributo TypeConverter:

[TypeConverter (typeof(ColorTypeConverter))]

public struct Color

{

…

}

ColorTypeConverter é uma classe pública, para que você possa experimentá-lo se você gostaria. Ele aceita definição de cores em vários formatos: Ele pode converter uma string como &quot;Accent&quot; strings para os valores Color.Default e Color.Accent, &quot;Blue&quot; para o valor Color.Blue, eo &quot;Default&quot; e. ColorTypeConverter também pode analisar seqüências que codificam valores vermelho-verde-azul, como &quot;# FF8080&quot;, que é um valor vermelho de 0xFF, um valor verde de 0x80, e um valor de 0x80 azul também.

Todos os valores RGB numéricos começam com um prefixo de sinal de número, mas que prefixo pode ser seguido com oito, seis, quatro, ou três dígitos hexadecimais para especificar valores de cor com ou sem um canal alfa. Aqui está a mais extensa sintaxe:

BackgroundColor=&quot;#aarrggbb&quot;

Cada uma das cartas representa um dígito hexadecimal, na ordem alfa (opacidade), vermelho, verde e azul. Para o canal alfa, tenha em mente que 0xFF é totalmente opaco e 0x00 é totalmente transparente. Aqui está a sintaxe sem um canal alfa:

BackgroundColor=&quot;#rrggbb&quot;

Neste caso, o valor de alfa é definido como 0xFF para opacidade total.

Dois outros formatos permitem que você especificar apenas um único dígito hexadecimal para cada canal:

BackgroundColor=&quot;#argb&quot;

BackgroundColor=&quot;#rgb&quot;

Nestes casos, o dígito é repetido para formar o valor. Por exemplo, # CF3 é a cor RGB 0xCC-0xFF0x33\. Esses formatos curtos raramente são utilizados.

A propriedade FontSize é do tipo double. Este é um pouco diferente de propriedades do tipo LayoutOptions e Color. Os LayoutOptions e Color são estruturas da parte de Xamarin.Forms, para que eles possam ser marcados com o C# TypeConverter atributo, mas não é possível marcar na estrutura .NET um Double como atributo TypeConverter apenas para tamanhos de fonte!

Em vez disso, a propriedade FontSize dentro da classe de etiqueta tem o atributo TypeConverter:

public class Label : View, IFontElement

{

…

[TypeConverter (typeof (FontSizeConverter))]

public double FontSize

{

…

}

…

}

O FontSizeConverter determina se a string passada a ele é um dos membros da enumeração NamedSize. Se não, FontSizeConverter assume o valor é um Double.

O último atributo definido no exemplo é FontAttributes. A propriedade FontAttributes é uma enumeração nomeado FontAttributes, e você já sabe que o analisador XAML trata tipos de enumeração automaticamente. No entanto, a enumeração FontAttributes tem um C# Flags atributo definido da seguinte forma:

[Flags]

public enum FontAttributes

{

None = 0,

Bold = 1,

Italic = 2

}

Por isso, o analisador XAML permite que vários membros separados por vírgulas:

FontAttributes=&quot;Bold,Italic&quot;

Esta demonstração da natureza mecânica do analisador XAML deve ser uma notícia muito boa. Isso significa que você pode incluir classes personalizadas em XAML, e essas classes podem ter propriedades de tipos personalizados, ou as propriedades podem ser de tipos padrão, mas permitir que os valores adicionais. Tudo que você precisa é que esses tipos ou propriedades com um Atributo C# TypeConverter e fornecer uma classe que deriva de TypeConverter.