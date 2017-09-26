## Dicionários de recursos {#dicion-rios-de-recursos}

Xamarin.Forms também suporta uma segunda abordagem para compartilhamento de objetos e valores, e enquanto essa abordagem tem um pouco mais de peso do que a extensão de marcação** x:Static**, é de alguma forma a causa de tudo mais versátil – os objetos compartilhados e os elementos visuais os usam.

**VisualElement **define uma propriedade chamada **Resources **que é do tipo **ResourceDictionary **- um dicionário com chaves string e valores do tipo objeto . Os itens podem ser adicionados a este dicionário direto no XAML, e eles podem ser acessados em XAML com as extensões de marcação **StaticResource **e **DynamicResource**.

Apesar de **x:Static **e **StaticResource **terem nomes um tanto similares, eles são bastante diferentes: **x:Static** faz referência a uma constante, um campo estático, uma propriedade estática, ou um membro de enumeração, enquanto **StaticResource **recupera um objeto de um **ResourceDictionary**. Enquanto a extensão de marcação **x:Static** é intrínseca ao XAML \(e, portanto, aparece em XAML com um prefixo x\), as extensões de marcação **StaticResource **e **DynamicResource **não são. Elas faziam parte da implementação XAML original no Windows Presentation Foundation, e **StaticResource **é também suportada em Silverlight, Windows Phone 7 e 8 e Windows 8 e 10.

Você vai usar **StaticResource **para a maioria dos propósitos e reservar **DynamicResource **para algumas aplicações especiais, então vamos começar com **StaticResource**.

## 



