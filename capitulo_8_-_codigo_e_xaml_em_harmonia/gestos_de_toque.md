## Gestos de Toque {#gestos-de-toque}

O **Buttom **em Xamarin.Forms responde aos toques dos dedos, mas você pode pegar gesto de toque de qualquer classe que derivado de **View**, incluindo** Label**, **BoxView**, e **Frame**. Estes eventos de toque não são construídos para a classe **View**, mas a classe **View **define uma propriedade chamada **GestureRecognizers**. Toques são habilitados adicionando um objeto na coleção **GestureRecognizers**_._ Uma instância de qualquer classe que deriva de **GestureRecognizer **podem ser adicionados a esta coleção, mas até agora só há uma: **TapGestureRecognizer**.

Veja como adicionar um **TapGestureRecognizer** a um **BoxView **em código:![](/assets/08-30-codegesture)**TapGestureRecognizer **também define uma propriedade **NumberOfTapsRequired **com um valor padrão de 1.

Para gerar eventos aproveitado, o objeto **View **deve ter sua propriedade **IsEnabled **definida como  , a propriedade **IsVisible **definido como **true **\(ou não será visível\), e sua propriedade **InputTransparent **definida como **false**. Estas são todas as condições padrão.

O manipulador **Tapped** é semelhante com _**handler**_ **Clicked** de **Buttom**:![](/assets/08-32-eventehandle)Normalmente, o argumento **sender **de um manipulador de eventos é o objeto que ativa o evento, neste caso seria o objeto **TapGestureRecognizer**. Entretanto, isso não seria de muita utilidade. Em vez disso, o argumento de sender para o manipulador Tappedé a _view_ a ser aproveitado, neste caso, o **BoxView**_._ Isso é muito mais útil!

Como o **Buttom**, **TapGestureRecognizer **também define propriedades de **Commande CommandParameter**; estes são usados ​​ao implementar o padrão de design **MVVM**, e eles são discutidos em próximos capítulos.

**TapGestureRecognizer **também define propriedades nomeadas **TappedCallback **e **TappedCallbackParameter **e um construtor que inclui um argumento **TappedCallback**. Estes são todos obsoletos e não devem ser utilizadas.

Em XAML, você pode anexar um **TapGestureRecognizer **a uma visão expressando a coleção **GestureRecognizers **como um elemento de propriedade:![](/assets/08-33-element)Como de costume, o XAML é um pouco mais curto do que o código equivalente.

Vamos fazer um programa que é inspirado em um dos primeiros jogos de computador autônomo.

A versão Xamarin.Forms deste jogo é chamado **MonkeyTap** porque é um jogo de imitação. Ele contém quatro elementos **BoxView**, coloridos em vermelho, azul, amarelo, verde. Quando o jogo começa, um dos elementos **BoxView **brilha, e você deve, em seguida, tocar naquele **BoxView**. **BoxView** pisca novamente seguido por outro, e agora você deve tocar em sequência. Em seguida, os dois flashes são seguidas por uma terceira e assim por diante. É um jogo bastante cruel porque não há nenhuma maneira de vencer. O jogo está cada vez mais difícil e mais difícil até que você perde.

O arquivo **MonkeyTapPage.xaml **instancia os quatro elementos **BoxView **e um botão no centro rotulado de "**Begin**".

![](/assets/08-33-tapmokey)![](/assets/08-30-tapmokey2)

Todos os quatro elementos **BoxView **aqui tem uma **TapGestureRecognizer **anexado, mas eles ainda possuem cores assinaladas. Isso é tratado no arquivo _code-behind_ porque as cores não vão ficar constantes. As cores precisam ser alteradas para o efeito intermitente.

O arquivo _code-behind_ começa com algumas constantes e campos variáveis. \(Você notará que um deles é sinalizada como protected; no próximo capítulo, a classe irá derivar dele e exigir o acesso a este campo alguns métodos são definidos como protected\).![](/assets/08-31-tapmokeycode)![](/assets/08-32-tapmokley2)

O construtor coloca todos os quatro elementos **BoxView **em um vetor; o que lhes permite ser referenciado por um índice simples que tem valores de 0, 1, 2 e 3. O método **InitializeBoxViewColors **define todos os elementos **BoxView **ao seu estado sem brilho ligeiramente escurecida.

O programa está agora espera que o usuário pressione o botão **Begin** para iniciar o primeiro jogo. O mesmo botão lida com replays, assim que inclui uma inicialização redundante das cores **BoxView**. O _**handler**_** **de Buttom também se prepara para construir a sequência de elementos **BoxView** piscavam desmarcando a lista sequence e chamando **StartSequence**:![](/assets/08-34-startsequence)

StartSequenceadiciona um novo número aleatório para a lista de sequence, inicializa sequenceIndexa 0, e inicia o timer.

No caso normal, o _handler_ de marcação timer é chamado para cada índice na lista de sequence e faz com que o **BoxView **correspondente pisque com uma chamada para **FlashBoxView**. O manipulador de cronômetro retorna false quando a sequência está no fim, indicando também definindo **awaitingTaps **que é hora para o usuário a imitar a sequência:![](/assets/08-35-imitarseq)

O **flash **é apenas um quarto de segundo de duração. O método **FlashBoxView** define primeiro a luminosidade para uma cor brilhante e cria um timer "one-shot", assim chamado porque o método temporizador de chamada de retorno \(aqui expressado como uma função lambda\) retorna falso e desliga o temporizador após a restauração da luminosidade da cor.

O manipulador **Tapped **para os elementos **BoxView **ignora o toque se o jogo já está encerrado \(o que só acontece com um erro do usuário\), e termina o jogo, se o usuário toca prematuramente sem esperar o programa para passar pela sequência. Caso contrário, ele apenas compara o **BoxView **tocado com o próximo na sequência , Brilha o **BoxView **se estiver correta , ou termina o jogo, caso não:

![](/assets/08-36-casonao)![](/assets/08-36senao2)

Se o usuário consegue acertar a sequência de todo o caminho, outra chamada para **StartSequence **acrescenta um novo índice para a lista de sequência e começa a tocar de novo. Eventualmente, porém, haverá uma chamada para **EndGame**, que colore todas as caixas cinzentas para enfatizar o fim, e reativa o botão para dar uma chance de tentar novamente.

Aqui está o programa depois que o Buttom foi clicado e escondido:

![](/assets/08-37-telas)

Eu sei eu sei. O jogo é uma chatice sem som.

Vamos aproveitar a oportunidade no próximo capítulo para corrigir isso.

