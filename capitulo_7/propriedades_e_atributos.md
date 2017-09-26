## Propriedades e atributos {#propriedades-e-atributos}

Aqui está uma Label Xamarin.Forms instanciada e inicializada em código, tanto quanto poderia parecer no construtor de uma classe de página:

![](/assets/07-01-label)

![](/assets/07-02-label1)

Aqui está uma Label muito semelhante instanciada e inicializada em XAML, que você pode ver imediatamente é mais conciso do que o código equivalente:![](/assets/07-03-labelxaml)Classe em Xamarin.Forms como **Label **se tornar elementos XML em XAML. Propriedades como Text, **IsVisible**, e o resto se tornar atributos XML em XAML.

Para ser instanciada em XAML, uma classe como a **Label **deve ter um **construtor sem parâmetros públicos**. \(No próximo capítulo, você vai ver que há uma técnica para passar argumentos para um construtor em XAML, mas é geralmente utilizado para fins especiais\). As propriedades definidas em XAML deve ter **assessores set públicos**. Por convenção, os espaços cercam um sinal de igual no código, mas não em XML \(ou XAML\), mas você pode usar tanto espaço em branco como você quer.

A concisão do XAML resulta principalmente da brevidade do exemplo de atributo valores for, o uso da palavra "**Grande**" em vez de uma chamada para o método **Device.GetNamedSize**. Essas abreviaturas não são incorporadas ao analisador XAML. O analisador XAML em vez disso é assistida por várias classes de conversor definidas especificamente para está finalidade.

Quando o analisador XAML encontra o elemento **Label**, ele pode usar o reflexo para determinar se o Xamarin.Forms tem uma classe chamada  **Label **e, nesse caso, é possível criar uma instância dessa classe. Agora ele está pronto para inicializar esse objeto. A propriedade de **Text **é do tipo string, e o valor do atributo é simplesmente atribuído a essa propriedade.

Porque XAML é XML, você pode incluir caracteres Unicode no texto usando a sintaxe XML padrão. Preceder o valor decimal Unicode com &\# \(ou o valor hexadecimal Unicode com &\#x\) e segui-lo com um ponto e vírgula:![](/assets/07-04-caracteres)

Esses são os valores Unicode para o travessão e símbolo do euro. Para forçar uma quebra de linha, use o caractere de avanço de linha **&\#x000A**, ou \(porque zeros à esquerda não são obrigados\)** &\#xA**, ou com o código decimal, **&\#10**.

As aspas têm um significado especial em XML, de modo a incluir esses caracteres em uma sequência de texto, use uma das entidades predefinidas padrão:

* &lt; for &lt;
* &gt; for &gt;
* &amp; for &
* &apos; for '
* &quot; for "

As entidades predefinidas HTML como **&nbsp**; não são suportados. Para um uso do espaço não separável **&\#xA0;** em vez disso.

Além disso, chaves \({ and }\) tem um significado especial em XAML. Se você precisa para começar um valor de atributo com uma chave esquerda, comece com um par de chaves \({}\) e, em seguida, a chave esquerda.

As propriedades **IsVisible **e **Opacity **da **Label **são do tipo **bool **e **double**, respectivamente, e estes são tão simples como se poderia esperar. O analisador XAML utiliza os métodos **Boolean.Parse** e **Double.Parse** para converter os valores de atributos. O método **Boolean.Parse **é caso insensível, mas os valores booleanos geralmente são capitalizados como** "True"** e** "False"** em XAML. O método **Double.Parse** é passado um argumento **CultureInfo.InvariantCulture**, de modo que a conversão não depende da cultura local do programador ou utilizador.

A propriedade **HorizontalTextAlignment **da **Label **é do tipo **TextAlignment**, que é uma enumeração. Para qualquer propriedade que é um tipo de enumeração, o analisador XAML usa o método **Enum.Parse** para converter a partir da cadeia de valor.

A propriedade **VerticalOptions **é do tipo **LayoutOptions**, uma estrutura. Quando o analisador XAML referência a estrutura **LayoutOptions **usando a reflexão, descobre que a estrutura tem um atributo de C\# definida:![](/assets/07-05-typeconverter)\(Atenção! Essa discussão envolve dois tipos de atributos: Atributos XML como XAlign e C\# atributos como este TypeConverter\)

O atributo **TypeConverter **é suportado por uma classe chamada **TypeConverterAttribute**. Este atributo **TypeConverter **em **LayoutOptions **faz referência a uma classe chamada **LayoutOptionsConverter**. Esta é uma classe privada para Xamarin.Forms, mas deriva de uma classe abstrata chamada pública **TypeConverter **que define métodos chamados **CanConvertFrom **e **ConvertFrom**. Quando o analisador XAML encontra esse atributo **TypeConverter**, ele instancia o **LayoutOptionsConverter**. Os **VerticalOptions** atribuir no XAML é atribuído a string "**Center**", de modo que o analisador XAML passa que "**Center**" string para o método **ConvertFrom **de **LayoutOptionsConverter**, e fora aparece um valor **LayoutOptions**. Isto é atribuído à propriedade **VerticalOptions **do objeto **Label**.

Da mesma forma, quando o analisador XAML encontra as propriedades **TextColor **e **BackgroundColor**, ele usa reflexão para determinar que essas propriedades são do tipo **Color**. A estrutura de cores também é adornada com um atributo **TypeConverter**:

![](/assets/07-06-color)Você pode criar uma instância de **ColorTypeConverter **e experimentar com o código se desejar. Ele aceita definições de cores em vários formatos: ele pode converter uma seqüência de caracteres como "**Azul**" para o valor **Color.Blue** e as strings "**Padrão**" e "**Acento**" para os valores** Color.Default** e **Color.Accent**.

O ColorTypeConverter também pode analisar seqüências de caracteres que codificam valores vermelho-verde-azul, como "\# **FF8080**", que é um valor vermelho de 0xFF, um valor verde de 0x80, e um valor de 0x80 azul também.

Todos os valores RGB numéricos começam com um prefixo de sinal de número, mas que prefixo pode ser seguido com oito, seis, quatro, ou três dígitos hexadecimais para especificar valores de cor com ou sem um canal alfa. Aqui está a mais extensa sintaxe:

![](/assets/07-08-rgb)

Cada uma das letras representa um dígito hexadecimal, na ordem alfa \(opacidade\), vermelho, verde e azul. Para o canal alfa, tenha em mente que 0xFF é totalmente opaco e 0x00 é totalmente transparente. Aqui está a sintaxe sem um canal alfa:

![](/assets/07-08-rgbalpha)

Neste caso, o valor de alfa é definido como 0xFF para opacidade total.

Dois outros formatos permitem que você especificar apenas um único dígito hexadecimal para cada canal:

![](/assets/07-09-rgb)

Nestes casos, o dígito é repetido para formar o valor. Por exemplo, \#CF3 é a cor RGB 0xCC-0xFF0x33. Esses formatos curtos raramente são utilizados.

A propriedade **FontSize **é do tipo **double**. Este é um pouco diferente de propriedades do tipo **LayoutOptions **e **Color**. Os **LayoutOptions **e **Color **são estruturas parte do Xamarin.Forms, para que eles possam ser marcados com o atributo C\# **TypeConverter **, mas não é possível marcar na estrutura .NET um **Double **como atributo **TypeConverter **apenas para tamanhos de fonte!

Em vez disso, a propriedade **FontSize **dentro da classe **Label **tem o atributo **TypeConverter**:

![](/assets/07-10-label)O **FontSizeConverter **determina se a string passada a ele é um dos membros da enumeração **NamedSize**. Se não, **FontSizeConverter **assume o valor é um **Double**.

O último atributo definido no exemplo é **FontAttributes**. A propriedade **FontAttributes **é uma enumeração nomeado **FontAttributes**, e você já sabe que o analisador XAML trata tipos de enumeração automaticamente. No entanto, a enumeração FontAttributes tem um  Flags  C\# atributo definido da seguinte forma:

![](/assets/07-11-flag)

Por isso, o analisador XAML permite que vários membros separados por vírgulas:

![](/assets/07-11-fontatribute][)

Esta demonstração da natureza mecânica do analisador XAML deve ser uma notícia muito boa. Isso significa que você pode incluir classes personalizadas em XAML, e essas classes podem ter propriedades de tipos personalizados, ou as propriedades podem ser de tipos padrão, mas permitir que os valores adicionais. Tudo que você precisa é que esses tipos ou propriedades com um Atributo C\# **TypeConverter **e fornecer uma classe que deriva de **TypeConverter**.

