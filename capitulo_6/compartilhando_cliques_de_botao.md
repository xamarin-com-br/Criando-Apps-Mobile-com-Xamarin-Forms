## Compartilhando cliques de botão {#compartilhando-cliques-de-bot-o}

Se um programa contém vários Button, cada Button pode ter seu próprio evento Clicked. Mas, em alguns casos, pode ser mais conveniente para vários Button compartilhar um evento Clicked em comum.

Considere um programa de calculadora. Cada um dos botões de 0 a 9 faz basicamente a mesma coisa, e ter 10 eventos Clicked separados para estes 10 botões—até mesmo se eles compartilham algum código em comum—simplesmente não faz muito sentido.

Você viu como o primeiro argumento para o evento Clicked pode ser convertido para um objeto do tipo Button. Mas como você sabe qual Button é?Uma abordagem é armazenar todos os objetos Button como campos e, em seguida, comparar o objeto Button disparador do evento com estes campos. O programa **TwoButtons** demonstra esta técnica. Este programa é similar ao programa anterior, mas com dois botões—um para adicionar objetos Label para o StackLayout, e o outro para removê-los. Os dois objetos Button são armazenados como campos para que o evento Clicked possa determinar qual deles disparou o evento:

Ambos os botões recebem um valor de HorizontalOptions e CenterAndExpand de modo que eles podem ser dispostos lado a lado na parte superior da tela utilizando um StackLayout horizontal:

Observe que quando o evento Clicked detecta removeButton, ele simplesmente chama o método RemoveAt na propriedade Children:

**loggerLayout.Children.RemoveAt(0);**

Mas o que acontece se não há nós? O método RemoveAt não vai levantar uma exceção? Isso não pode acontecer! Quando o programa **TwoButtons** começa, a propriedade IsEnabled do remove-Button é inicializada como false. Quando um botão está desabilitado, ele apresenta uma aparência fraca que faz com que ele parece ser não funcional, e ele realmente não está funcionando. Ele não fornece resposta para o usuário e não dispara eventos Clicked. Perto do final do evento Clicked, a propriedade IsEnabled do removeButton é definido como true somente se o loggerLayout tem pelo menos um nó. Isso ilustra uma boa prática: se o seu código precisa determinar se um evento Clicked de botão é válido, é provavelmente melhor evitar cliques inválidos de botão desativando-os.