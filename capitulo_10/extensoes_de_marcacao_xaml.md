## Extensões de marcação XAML {#extens-es-de-marca-o-xaml}

No código, você pode definir uma propriedade em uma variedade de maneiras diferentes a partir de uma variedade de fontes diferentes:

**triangle.Angle1 = 45;**

**triangle.Angle1 = * 180 radianos / Math.PI;**

**triangle.Angle1 = ângulos [i];**

**triangle.Angle1 animator.GetCurrentAngle = ();**

Se este Angle1 é uma propriedade double, Tudo que é necessário é que a fonte seja um double ou de outra forma provenha um valor numérico que é conversível para um double.

Na marcação, no entanto, uma propriedade do tipo double normalmente só pode ser definida a partir de uma seqüência de caracteres que se qualifica como um argumento válido para Double.Parse. A única exceção que você já viu até agora é quando a propriedade alvo flagueada com um atributo TypeConverter, tal como a propriedade FontSize.

Seria desejável se XAML fosse mais flexível - se você pudesse definir uma propriedade de outras fontes do que seqüências de texto explícitas. Por exemplo, suponha que você queira definir uma outra maneira de setar uma propriedade do tipo Color, Talvez usando uma Matiz ,Saturação e Luminosidade, mas sem o incômodo do elemento x: FactoryMethod. Apenas de improviso, não parece possível. O analisador XAML espera que qualquer valor definido como um atributo do tipo Color é uma string aceitável para a classe ColorTypeConverter.

O objetivo das extensões de marcação XAML é contornar esta restrição aparente. Tenha certeza de que Extensões de marcação XAML não são extensões para XML. XAML é sempre XML legal. As extensões de marcação XAML são extensões apenas no sentido de que eles estendem as possibilidades de configurações de atributo na marcação. Uma extensão de marcação fornece essencialmente um valor de um tipo particular, sem necessariamente ser uma representação de texto de um valor.