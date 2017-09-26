## A infra-estrutura de código {#a-infra-estrutura-de-c-digo}

Estritamente falando, uma extensão de marcação XAML é uma classe que implementa **ImarkupExtension**, Que é uma interface pública definida no **Xamarin.Forms.Core** assembly , mas com o namespace **Xamarin.Forms.Xam**l:

**public interface IMarkupExtension{**

**object ProvideValue \(IServiceProvider serviceProvider\);**

**}**

Como o nome sugere, **ProvideValue **é o método que fornece um valor para um atributo XAML. **IserviceProvider **faz parte das bibliotecas de classe base do .NET e definido no namespace **System**:

**public interface IServiceProvider**

**{**

**object GetService \(Type type\);**

**}**

Obviamente, esta informação não fornece nada além de uma dica sobre como escrever extensões de marcação personalizadas, e na verdade, podem ser complicadas. \(Você verá um exemplo brevemente e outros exemplos mais adiante neste livro.\) Felizmente, Xamarin.Forms fornece várias extensões de marcação valiosas para você. Estas se dividem em três categorias:

* Extensões de marcação que são parte da especificação XAML de 2009. Estas aparecem em arquivos XAML com o habitual prefixo x e são:

* **x: Static**
* **x: Reference**
* **x: Type**
* **x: nulo**
* **x: Array**

Estes são implementados em classes que consistem do nome da extensão de marcação com a palavra Extension anexado, são exemplos as classes **StaticExtension **e **ReferenceExtension**. Estas classes são definidas na assembly Xamarin.Forms.Xaml.

As seguintes extensões de marcação se originaram no Windows Presentation Foundation \(WPF\) e, com excessão da DynamicResource, são suportados por outras implementaçãoes de XAML da Microsoft, incluindo Silverlight, Windows Phone 7 e 8 e Windows 8 e 10:

* **StaticResource**
* **DynamicResource**
* **Binding**

A classe **DynamicResourceExtension **é pública; as **StaticResourceExtension **e **BindingExtension **não, mas estão disponíveis para seu uso em arquivos XAML porque são acessíveis ao analisador XAML. Há apenas uma extensão de marcação que é exclusiva para Xamarin.Forms: a classe **ConstraintExpression **usada com **RelativeLayout**.

Embora seja possível brincar com classes públicas de marcação de extensão em código, elas realmente só fazem sentido em XAML.

