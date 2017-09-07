## 5. Centralizar o texto com o label {#5-centralizar-o-texto-com-o-label}

O Label é destinado para exibir o texto até um parágrafo de comprimento. Muitas vezes é desejável para controlar como as linhas de texto são alinhadas horizontalmente: alinhado à esquerda, à direita justificada, ou centralizado.

O Label view define uma propriedade xalign para o efeito e também uma propriedade YAlign para o texto posicionamento verticalmente. Ambas as propriedades são definidas para um membro da enumeração TextAlignment, que tem membros nomeados em Iniciar, Centro, e acabam de ser versátil o suficiente para o texto que vai da direita para a esquerda ou de cima para baixo. Para Inglês e outros idiomas europeus, Start significa esquerda ou superior e End significa direita ou inferior.

Para esta solução definitiva para o problema barra de status iOS, definir xalign e YAlign para TextAlignment.Center:

```
public class GreetingsPage : ContentPage
{
 public GreetingsPage()
 {
 Content = new Label
 {
 Text = "Greetings, Xamarin.Forms!",
 HorizontalTextAlignment = TextAlignment.Center,
 VerticalTextAlignment = TextAlignment.Center
 };
 }
}
```

Visualmente, o resultado com esta única linha de texto é o mesmo que definir HorizontalOptions e VerticalOptions para Center, e você também pode usar várias combinações destas propriedades para posicionar o texto em um dos nove locais diferentes em torno da página. No entanto, estas duas técnicas para centralizar o texto são realmente muito diferentes, como você verá no próximo capítulo.

