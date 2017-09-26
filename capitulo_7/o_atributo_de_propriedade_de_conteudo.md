## O atributo de propriedade de conteúdo {#o-atributo-de-propriedade-de-conte-do}

O arquivo XAML no programa **ScaryColorList** é realmente um pouco mais do que ele precisa ser. você pode eliminar as **labels ContentPage.Conten**t, todas as tags **StackLayout**.**Children**, e todas as tag **Frame**.**Content**, e o programa irá funcionar da mesma forma:

![](/assets/07-40-Scary02)![](/assets/07-40-sacray03)

Parece muito mais limpo agora. O único elemento de propriedade esquerda é para a propriedade **Padding **de **ContentPage**.

Tal como acontece com quase toda sintaxe XAML, esta eliminação de alguns elementos de propriedade é apoiada pelas classes subjacentes. Em cada classe usado em XAML é permitido definir uma propriedade como uma propriedade _**content**_** **\(às _vezes_ também chamado de propriedade _padrão_ _da_ _classe_\). Para esta propriedade de conteúdo, as tags do elemento não são necessárias, e qualquer conteúdo XML dentro das tags de início e fim é automaticamente atribuído a esta propriedade. Muito convenientemente, a propriedade do conteúdo é **Content ContentPage**, a propriedade de conteúdo de **StackLayout **é **Children**, e a propriedade de conteúdo do **Frame **é **content**.

Estas propriedades de conteúdo são documentadas, mas você precisa saber para onde olhar. Uma classe específica é propriedade de conteúdo usando o **ContentPropertyAttribute**. Se esse atributo está ligado a uma classe, ele aparece na documentação on-line Xamarin.Forms API, juntamente com a declaração de classe. É assim que aparece na documentação para ContentPage:

> **\[Xamarin.Forms.ContentProperty\("Content"\)\] public class ContentPage : Page**

Se ler em voz alta, soa um pouco redundante: A propriedade **Content **é a propriedade de conteúdo de **ContentPage**.

A declaração para a classe **Frame **é semelhante:

> **\[Xamarin.Forms.ContentProperty\("Content"\)\] public class Frame : ContentView**

**StackLayout **não tem um atributo **ContentProperty **aplicado, mas **StackLayout **deriva de** Layout&lt;View&gt;**, e **Layout&lt;T&gt;** tem um atributo **ContentProperty**:

> **\[Xamarin.Forms.ContentProperty\("Children"\)\]**
>
> **public abstract class Layout&lt;T&gt; : Layout, IViewContainer&lt;T&gt; where T : Xamarin.Forms.View**

O atributo **ContentProperty** é herdado pelas classes que derivam de **Layout &lt;T&gt;**, então **Children **é a propriedade de conteúdo de **StackLayout**.

Certamente, não há nenhum problema se você incluir os elementos de propriedade quando não são necessários, mas na maioria dos casos, eles não serão mais exibidos nos programas de exemplo deste livro.

