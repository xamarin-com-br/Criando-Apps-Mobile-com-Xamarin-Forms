## Sintaxe Propriedade – Elemento {#sintaxe-propriedade-elemento}

Aqui está alguns exemplos em C\# que são semelhantes ao código **FramedText** no Capítulo 4. Em um comando que instancia um quadro e um rótulo e define o rótulo para o Content propriedade da moldura:

![](/assets/07-12-frame)

Mas quando você começa a duplicar este em XAML, você pode se tornar um pouco frustrado no ponto onde você definir o atributo **Content**:

![](/assets/07-13-outline)![](/assets/07-13-outline1)

Como pode esse atributo **Content **pode ser definido como todo um objeto **Label**?

A solução para este problema é a característica mais fundamental de sintaxe XAML. O primeiro passo é o de separar o marcador do **Frame **em marcas de início e de fim:

![](/assets/07-14-frame)

Dentro dessas marcas, adicionar mais duas marcas que consistem de o elemento \(**Frame**\) e a propriedade que você deseja definir \(**Content**\), ligada com um período de:

![](/assets/07-16-frame)Agora coloque o **Label** entre estas tags : ![](/assets/07-17-framelabel)

Essa sintaxe é como você seta a **Label **e o **Content **no **Frame**.

Você pode se perguntar se esse recurso XAML viola regras de sintaxe XML. Isso não. O período não tem nenhum significado especial em XML, então **Frame**.Content é uma tag XML perfeitamente válida. No entanto, XAML impõe suas próprias regras sobre essas tags: As tags **Frame.Content** devem aparecer dentro de **tags **de **Frame**, e nenhum atributo pode ser definido na **tag Frame.Content**. O objeto definido para a propriedade de **Content **aparece como o conteúdo XML dessas tags.

Uma vez que essa sintaxe é introduzida, alguma terminologia torna-se necessária. No trecho XAML final mostrada acima:

* **Frame **e **Label **são objetos C\# expressas como elementos XML. Eles são chamados de elementos do objeto.
* **OutlineColor**, **HorizontalOptions**, **VerticalOptions**, e Text são propriedades do C\# expressas como atributos XML. Eles são chamados de atributos de propriedades.
* **Frame.Content **é uma propriedade C\# expressa como um elemento XML, e por isso é chamado de elemento de propriedade.

Elementos de propriedade são muito comuns em XAML da vida real. Você vai ver inúmeros exemplos neste capítulo e os capítulos futuros, e em breve você vai encontrar elementos de propriedade tornando-se uma segunda natureza para o seu uso do XAML. Mas cuidado: Às vezes, os desenvolvedores devem se lembrar tanto que nós esquecemos o básico. Mesmo depois de ter sido usando XAML por um tempo, você provavelmente vai encontrar uma situação em que não parece possível definir um determinado objeto a uma propriedade particular. A solução é muitas vezes um elemento de propriedade.

Você também pode usar a sintaxe da propriedade de elemento para propriedades mais simples, por exemplo:

![](/assets/07-18-frame-horizontal)

Agora as propriedades **VerticalOptions **e **OutlineColor **são do Frame e a propriedade **Text **da **Label **têm todos se tornam elementos de propriedade. O valor desses atributos é o conteúdo do elemento de propriedade sem aspas.

Claro, isso não faz muito sentido para definir essas propriedades como elementos da propriedade. É desnecessariamente detalhada. Mas ele funciona como deveria.

Vamos ir um pouco mais longe: Em vez de definir **HorizontalOptions **para "**Center**" \(correspondente à propriedade **LayoutOptions**.**Center** estática\), você pode expressar **HorizontalOptions **como um elemento da propriedade e configurá-lo para um valor LayoutOptions com suas propriedades individuais estabelecidos:

![](/assets/07-19-frame-optino)![](/assets/07-19-frame-option1)E você pode ver expressadas essas propriedades do LayoutOption com propriedades do elemento:![](/assets/07-20-layoutoptions)

Você não pode definir a mesma propriedade como um atributo de propriedade e um elemento de propriedade. Que está definindo a propriedade duas vezes, e não é permitido. E lembre-se que nada mais pode aparecer nas marcas de elemento propriedade-. O valor a ser definido para a propriedade é sempre o conteúdo XML dessas tags.

Agora você deve saber como usar um **StackLayout **em XAML. Primeiro expressar a propriedade **Children **como o elemento de propriedade **StackLayout**.**Children **e, em seguida, incluir os filhos da **StackLayout **como conteúdo XML das tags propriedade de elementos. Aqui está um exemplo onde o primeiro StackLayout tem um outro **StackLayout **com uma orientação horizontal:

![](/assets/07-21-stacklayout)![](/assets/07-21-staclayot2)

Cada **StackLayout **horizontal tem um **BoxView **com uma cor e uma **Label** com esse nome cor.

Claro, a marcação repetitiva aqui parece um pouco assustador! E se você quiser exibir 16 cores? Ou 140? Você pode ter sucesso em primeiro lugar com um monte de copiar e colar, mas se você, então, necessário para refinar o visual um pouco, você estaria em má forma. No código que você faria isso em um loop, mas XAML não tem essa característica.

Quando a marcação ameaça de ser excessivamente repetitivo, você sempre pode usar o código. Definindo alguns de uma interface de usuário em XAML eo restante em código é perfeitamente razoável. Mas há outras soluções, como você verá em capítulos posteriores.

