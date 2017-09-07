## 2. Incluir preenchimento apenas para IOS {#2-incluir-preenchimento-apenas-para-ios}

Uma das vantagens da abordagem de projeto recurso compartilhado \(SAP\) é que as classes no projeto são extensões dos projetos de aplicativos, assim você pode usar diretivas de compilação condicional.

Vamos tentar fazer isso. Vamos precisar de uma nova solução chamada GreetingsSap com base no modelo SAP, e uma nova classe de página no projeto GreetingsSap chamado GreetingsSapPage. Essa classe tem esta aparência:

```
namespace GreetingsSap
{
 public class GreetingsSapPage : ContentPage
 {
 public GreetingsSapPage ()
 {
 Content = new Label
 {
 Text = "Greetings, Xamarin.Forms!"
 };
#if __IOS__
 Padding = new Thickness(0, 20, 0, 0);
#endif

 }
 }
}
```

A diretiva \#if referência o símbolo de compilação condicional **IOS**, portanto, a propriedade Padding é definida somente para o projeto iOS. Os resultados são parecidos com este:

![](/assets/2.4-contentepage-ios.PNG)

No entanto, esses símbolos de compilação condicional afetam somente a compilação do programa, para que eles não têm nenhum efeito em um PCL. Existe uma maneira para um projeto PCL para incluir estofamento diferente para as três plataformas?

