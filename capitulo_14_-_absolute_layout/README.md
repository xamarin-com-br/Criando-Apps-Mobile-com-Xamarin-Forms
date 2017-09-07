# Capítulo 14 - Absolute Layout {#cap-tulo-14-absolute-layout}

No Xamarin.Forms, o conceito de disposição abrange todas as maneiras que várias interfaces podem ser apresentadas na tela. Abaixo é apresentada a hierarquia com todas as classes que derivam de Layout

*   System.Object
    *   BindableObject
        *   Element
            *   VisualElement
                *   View

Layout

ContentView

Frame

ScrollView

Layout&lt;T&gt;

AbsoluteLayout

Grid

RelativeLayout

StackLayout

Você já viu o ContentView, Frame e ScrollView (todos possuem a propriedade Content que você pode definir para um elemento filho), e também o StackLayout, que herda a propriedade Children de Layout&lt;T&gt; e exibe o elemento filho em uma pilha vertical ou horizontal. O Grid e RelativeLayout implementam modelos de layout um pouco mais complexos, que serão explorados nos próximos capítulos. O recurso AbsoluteLayout é o principal assunto desse capítulo.

Primeiramente, a classe AbsoluteLayout parece implementar um modelo de layout bastante primitivo, em que remete aos não tão bons velhos tempos de interfaces gráficas, onde os programadores eram obrigados a dispor cada elemento na tela individualmente. No entanto, você vai descobrir que o AbsoluteLayout incorpora um posicionamento proporcional, que ajuda a transformar esse layout antigo em um modelo mais moderno.

Com o AbsoluteLayout, muitas das regras sobre layout que você aprendeu até agora não se aplicam: as propriedades HorizontalOptions e VerticalOptions que são tão importantes quando uma View é filho de um ContentPage ou StackLayou não tem absolutamente nenhum efeito quando a View é filha de um AbsoluteLayout. O programa em vez de atribuir a cada elemento filho de um AbsoluteLayout um local especifico nas coordenadas do dispositivo. O elemento filho também pode ser atribuído a um tamanho especifico ou permitido ao tamanho de si mesmo.

Você pode usar o AbsoluteLayout em seu código ou no XAML. Para o XAML, a classe faz uso do recurso suportado pelo BindableObject e o BindableProperty. Essa é a propriedade que permite fazer o bind com um tipo especial de propriedade, que pode ser definido em uma instancia da classe que não seja a classe que define a propriedade.