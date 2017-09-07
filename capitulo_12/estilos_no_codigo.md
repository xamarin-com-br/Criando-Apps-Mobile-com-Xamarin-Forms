## Estilos no código {#estilos-no-c-digo}

Embora os estilos são mais os mais definidos e usados em XAML, você deve saber como eles se parecem quando definidos e usados no código. Aqui está a classe de página para o projeto &quot;BasicStyleCode&quot; somente de código. O construtor da classe &quot;BasicStyleCodePage&quot; usa a sintaxe objeto de inicialização para imitar a sintaxe &quot;XAML&quot; na definição do objeto Estilo e aplicá-lo para três botões:

É muito mais evidente no código que em &quot;XAML&quot; a propriedade &quot;property&quot; do &quot;Setter&quot; é do tipo &quot;BindableProperty&quot;.

Os dois primeiros objetos &quot;Setter&quot; neste exemplo são inicializados com os objetos &quot;BindableProperties&quot; nomeados &quot;View.HorizontalOptionsProperty&quot; e &quot;View.VerticalOptionsProperty&quot;. Você poderia usar &quot;Button.HorizontalOptionsProperty&quot; e &quot;Button.VerticalOptionsProperty&quot; em vez disso porque &quot;Button&quot; herda essas propriedades de &quot;View&quot;. Ou você poderia mudar o nome da classe para qualquer outra classe que deriva de &quot;View&quot;.

Como de costume, o uso de um &quot;ResourceDictionary&quot; no código parece inútil. Você poderia eliminar o dicionário e apenas atribuir os objetos de estilo diretamente para as propriedades de estilo dos botões. Contudo, mesmo em código, o estilo é uma maneira conveniente de agrupar todas as configurações de propriedade juntas em um pacote compacto.