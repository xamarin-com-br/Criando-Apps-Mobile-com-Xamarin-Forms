## 4. Centralizar o label com a página {#4-centralizar-o-label-com-a-p-gina}

O problema com o texto sobrepondo a barra de status iOS ocorre apenas porque o texto, por padrão, é exibido no canto superior esquerdo. É possível centralizar o texto na página?

Xamarin.Forms suporta um número de instalações para facilitar o layout sem exigir que o programa de cálculos por forma envolvendo os tamanhos e coordenadas. A classe View define duas propriedades, HorizontalOptions e VerticalOptions nomeados, que especificam como uma view, deve ser posicionada em relação ao respectivo elemento principal \(neste caso o ContentPage\). Essas duas propriedades são de LayoutOptions tipo, uma estrutura com oito campos estáticos públicos de somente leitura que retornam valores LayoutOptions:

* **Start**
* **Center**
* **End**
* **Fill**
* **StartAndExpand**
* **CenterAndExpand**
* **EndAndExpand**
* **FillAndExpand**

A estrutura LayoutOptions também define duas propriedades da ocorrência que lhe permitem formular estas mesmas combinações:

* Uma propriedade Alinhamento do tipo LayoutAlignment, uma enumeração com quatro membros: Iniciar, Centro, End, e Fill.
* Uma propriedade Expande do tipo bool.

A explicação mais completa de como funcionam estes, espera por você no Capítulo 4, "Rolagem a pilha", mas por agora você pode definir as propriedades HorizontalOptions e VerticalOptions do rótulo para um dos campos estáticos definidos por valores LayoutOptions. Para HorizontalOptions, a palavra significa Iniciar esquerda e End significa direita; para VerticalOptions, Start significa superior e End significa inferior.

Dominar o uso das propriedades HorizontalOptions e VerticalOptions é uma parte importante de adquirir habilidade no sistema de layout Xamarin.Forms, mas aqui está um exemplo simples que posiciona a etiqueta no centro da página:

```
public class GreetingsPage : ContentPage
{
 public GreetingsPage()
 {
 Content = new Label
 {
 Text = "Greetings, Xamarin.Forms!",
 HorizontalOptions = LayoutOptions.Center,
 VerticalOptions = LayoutOptions.Center
 };
 }
}
```

Aqui está como é exibido:

![](/assets/2.5-labelios.PNG)

Esta é a versão do programa **Greetings** que está incluído no código de exemplo para este capítulo. Você pode usar várias combinações de HorizontalOptions e VerticalOptions para posicionar o texto em qualquer um dos nove lugares em relação à página.

