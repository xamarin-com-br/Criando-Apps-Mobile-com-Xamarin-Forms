## XML e Layout Absoluto {#xml-e-layout-absoluto}

Como você viu, você pode posicionar e dimensionar um filho de um AbsoluteLayout, usando um dos métodos Add disponíveis na coleção Children ou definindo uma propriedade anexada por meio de uma chamada de método estático.

Mas como na terra você definir a posição e tamanho das crianças AbsoluteLayout em XAML?

Uma sintaxe especial está envolvida. Essa sintaxe é ilustrada por esta versão XAML do anterior programa **AbsoluteDemo**, chamado **AbsoluteXamlDemo**:

O arquivo code-behind contém apenas uma chamada InitializeComponent.

Aqui está a primeira BoxView

Em XAML, uma propriedade bindable anexado é um atributo que consiste em um nome de classe (AbsoluteLayout) e um nome de propriedade (LayoutBounds) separados por um ponto. Sempre que você vê como um atributo, é uma propriedade bindable anexado. Essa é a única aplicação desta sintaxe de atributo.

Em resumo, as combinações de nomes de classes e nomes de propriedade só aparecem em XAML em três contextos específicos: Se eles aparecem como elementos, eles são elementos de propriedade. Se eles aparecem como atributos, estão ligadas propriedades vinculáveis. E o único outro contexto para um nome de classe e propriedade de nome é um argumento para um x: extensão de marcação estática.

Neste caso, o atributo é definido como quatro números separados por vírgulas. Você também pode expressar AbsoluteLayout.LayoutBounds como um elemento de propriedade:

Estes quatros números são analisados pelo BoundsTypeConverter e não o RectangleTypeConverter porque o BoundsTypeConverter permite o uso de AutoDimensionar para as partes de largura e altura. Você pode ver os argumentos AutoSize mais tarde no arquivo XAML:

Ou você pode deixá-los fora:

A coisa estranha sobre propriedades vinculava á anexados é que você especificar no XAML que eles realmente não existem! Não há nenhum campo, propriedade ou método em AbsoluteLayout chamado LayoutBounds. Há certamente um campo só de leitura estática pública do tipo BindableProperty chamado LayoutBoundsProperty, e há métodos estáticos públicos nomeados SetLayoutBounds e GetLayoutBounds, mas não há nada chamado LayoutBounds. O analisador XAML reconhece a sintaxe como uma referência a uma propriedade bindable anexado e, em seguida, olha para LayoutBoundsProperty na classe AbsoluteLayout. De lá, ele pode chamar SetValue na visão alvo com que BindableProperty objeto juntamente com o valor do BoundsTypeConverter.

A série tabuleiro de xadrez de programas parece um candidato improvável para a duplicação em XAML porque o arquivo precisaria de 32 casos de BoxView sem o benefício de loops. No entanto, o programa ChessboardXaml mostra como especificar duas propriedades de BoxView em um estilo implícita, incluindo os ligados AbsoluteLayout.LayoutFlags propriedade bindable.

Sim, é um monte de elementos BoxView individuais, mas você não pode argumentar com a limpeza do arquivo. O arquivo code-behind simplesmente ajusta a relação de aspecto: