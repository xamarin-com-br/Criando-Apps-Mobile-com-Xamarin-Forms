## Sintaxe Propriedade – Elemento {#sintaxe-propriedade-elemento}

Aqui está alguns C# que é semelhante ao código **FramedText** no Capítulo 4\. Em um comando que instancia um quadro e um rótulo e define o rótulo para o Content propriedade da moldura:

new Frame

{

OutlineColor = Color.Accent,

HorizontalOptions = LayoutOptions.Center,

VerticalOptions = LayoutOptions.Center,

Content = new Label

{

Text = &quot;Greetings, Xamarin.Forms!&quot;

}

};

Mas quando você começa a duplicar este em XAML, você pode se tornar um pouco frustrado no ponto onde você definir o atributo Content:

&lt;Frame OutlineColor=&quot;Accent&quot; HorizontalOptions=&quot;Center&quot; VerticalOptions=&quot;Center&quot;&gt;

&lt;/Frame&gt;

Como pode esse atributo Content ser definido como todo um objeto Label?

A solução para este problema é a característica mais fundamental de sintaxe XAML. O primeiro passo é o de separar o marcador do Frame em marcas de início e de fim:

&lt;Frame OutlineColor=&quot;Accent&quot; HorizontalOptions=&quot;Center&quot; VerticalOptions=&quot;Center&quot;&gt;

&lt;/Frame&gt;

Dentro dessas marcas, adicionar mais duas marcas que consistem de o elemento (Frame) ea propriedade que você deseja definir (Content), ligada com um período de:

&lt;Frame OutlineColor=&quot;Accent&quot; HorizontalOptions=&quot;Center&quot; VerticalOptions=&quot;Center&quot;&gt;

&lt;Frame.Content&gt;

&lt;/Frame.Content&gt;

&lt;/Frame&gt;

Agora coloque a Label dentro dessas tags:

&lt;Frame OutlineColor=&quot;Accent&quot; HorizontalOptions=&quot;Center&quot; VerticalOptions=&quot;Center&quot;&gt;

&lt;Frame.Content&gt;

&lt;Label Text=&quot;Greetings, Xamarin.Forms!&quot; /&gt;

&lt;/Frame.Content&gt;

&lt;/Frame&gt;

Essa sintaxe é como você seta a Label e o Content no Frame.

Você pode se perguntar se esse recurso XAML viola regras de sintaxe XML. Isso não. O período não tem nenhum significado especial em XML, então Frame.Content é uma tag XML perfeitamente válido. No entanto, XAML impõe suas próprias regras sobre essas tags: As tags Frame.Content deve aparecer dentro de tags de Frame, e nenhum atributo pode ser definido na tag Frame.Content. O objeto definido para a propriedade de Content aparece como o conteúdo XML dessas tags.

Uma vez que essa sintaxe é introduzida, alguma terminologia torna-se necessário. No trecho XAML final mostrada acima:

*   Frame e Label são objetos C# expressas como elementos XML. Eles são chamados de elementos do objeto.
*   OutlineColor, HorizontalOptions, VerticalOptions, e Text são propriedades de C# expressas como atributos XML. Eles são chamados de atributos de propriedades.
*   Frame.Content é uma propriedade C# expressa como um elemento XML, e por isso é chamado de elemento de propriedade.

Elementos de propriedade são muito comuns em XAML da vida real. Você vai ver inúmeros exemplos neste capítulo e os capítulos futuros, e em breve você vai encontrar elementos de propriedade tornando-se uma segunda natureza para o seu uso do XAML. Mas cuidado: Às vezes, os desenvolvedores devem se lembrar tanto que nós esquecemos o básico. Mesmo depois de ter sido usando XAML por um tempo, você provavelmente vai encontrar uma situação em que não parece possível definir um determinado objeto a uma propriedade particular. A solução é muitas vezes um elemento de propriedade.

Você também pode usar a sintaxe da propriedade de elemento para propriedades mais simples, por exemplo:

&lt;Frame HorizontalOptions=&quot;Center&quot;&gt;

&lt;Frame.VerticalOptions&gt; Center

&lt;/Frame.VerticalOptions&gt;

&lt;Frame.OutlineColor&gt; Accent

&lt;/Frame.OutlineColor&gt;

&lt;Frame.Content&gt;

&lt;Label&gt;

&lt;Label.Text&gt;

Greetings, Xamarin.Forms!

&lt;/Label.Text&gt;

&lt;/Label&gt;

&lt;/Frame.Content&gt;

&lt;/Frame&gt;

Agora as propriedades VerticalOptions e OutlineColor são do Frame e a propriedade Text da Label têm todos se tornam elementos de propriedade. O valor desses atributos é o conteúdo do elemento de propriedade sem aspas.

Claro, isso não faz muito sentido para definir essas propriedades como elementos da propriedade. É desnecessariamente detalhada. Mas ele funciona como deveria.

Vamos ir um pouco mais longe: Em vez de definir HorizontalOptions para &quot;Center&quot; (correspondente à propriedade LayoutOptions.Center estática), você pode expressar HorizontalOptions como um elemento da propriedade e configurá-lo para um valor LayoutOptions com suas propriedades individuais estabelecidos:

&lt;Frame&gt;

&lt;Frame.HorizontalOptions&gt;

&lt;LayoutOptions Alignment=&quot;Center&quot; Expands=&quot;False&quot; /&gt;

&lt;/Frame.HorizontalOptions&gt;

&lt;Frame.VerticalOptions&gt; Center

&lt;/Frame.VerticalOptions&gt;

&lt;Frame.OutlineColor&gt; Accent

&lt;/Frame.OutlineColor&gt;

&lt;Frame.Content&gt;

&lt;Label&gt;

&lt;Label.Text&gt;

Greetings, Xamarin.Forms!

&lt;/Label.Text&gt;

&lt;/Label&gt;

&lt;/Frame.Content&gt;

&lt;/Frame&gt;

E você pode expressar essas propriedades de LayoutOptions como elementos de propriedade:

&lt;Frame&gt;

&lt;Frame.HorizontalOptions&gt;

&lt;LayoutOptions&gt;

&lt;LayoutOptions.Alignment&gt; Center

&lt;/LayoutOptions.Alignment&gt;

&lt;LayoutOptions.Expands&gt; False

&lt;/LayoutOptions.Expands&gt;

&lt;/LayoutOptions&gt;

&lt;/Frame.HorizontalOptions&gt;

…

&lt;/Frame&gt;

Você não pode definir a mesma propriedade como um atributo de propriedade e um elemento de propriedade. Que está definindo a propriedade duas vezes, e não é permitido. E lembre-se que nada mais pode aparecer nas marcas de elemento propriedade-. O valor a ser definido para a propriedade é sempre o conteúdo XML dessas tags.

Agora você deve saber como usar um StackLayout em XAML. Primeiro expressar a propriedade Children como o elemento de propriedade StackLayout.Children e, em seguida, incluir os filhos da StackLayout como conteúdo XML das tags propriedade de elementos. Aqui está um exemplo onde o primeiro StackLayout tem um outro StackLayout com uma orientação horizontal:

&lt;StackLayout&gt;

&lt;StackLayout.Children&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;&gt;

&lt;StackLayout.Children&gt;

&lt;BoxView Color=&quot;Red&quot; /&gt;

&lt;Label Text=&quot;Red&quot;

VerticalOptions=&quot;Center&quot; /&gt;

&lt;/StackLayout.Children&gt;

&lt;/StackLayout&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;&gt;

&lt;StackLayout.Children&gt;

&lt;BoxView Color=&quot;Green&quot; /&gt;

&lt;Label Text=&quot;Green&quot;

VerticalOptions=&quot;Center&quot; /&gt;

&lt;/StackLayout.Children&gt;

&lt;/StackLayout&gt;

&lt;StackLayout Orientation=&quot;Horizontal&quot;&gt;

&lt;StackLayout.Children&gt;

&lt;BoxView Color=&quot;Blue&quot; /&gt;

&lt;Label Text=&quot;Blue&quot;

VerticalOptions=&quot;Center&quot; /&gt;

&lt;/StackLayout.Children&gt;

&lt;/StackLayout&gt;

&lt;/StackLayout.Children&gt;

&lt;/StackLayout&gt;

Cada StackLayout horizontal tem um BoxView com uma cor e uma Label com esse nome cor.

Claro, a marcação repetitiva aqui parece um pouco assustador! E se você quiser exibir 16 cores? Ou 140? Você pode ter sucesso em primeiro lugar com um monte de copiar e colar, mas se você, então, necessário para refinar o visual um pouco, você estaria em má forma. No código que você faria isso em um loop, mas XAML não tem essa característica.

Quando a marcação ameaça de ser excessivamente repetitivo, você sempre pode usar o código. Definindo alguns de uma interface de usuário em XAML eo restante em código é perfeitamente razoável. Mas há outras soluções, como você verá em capítulos posteriores.