## Incluindo propriedades Bindable {#incluindo-propriedades-bindable}

Se quiséssemos que este tabuleiro seja o maior quanto possível na tela, nós precisamos adicionar os elementos BoxView ao AbsoluteLayout durante o evento SizeChanged para a página, ou o evento SizeChanged teria de encontrar alguma maneira de mudar a posição e o tamanho do BoxView nos elementos filhos.

São possíveis ambas as opções, mas o segundo é preferido pois podemos preencher a coleção de elementos filhos do AbsoluteLayout apenas uma vez no construtor do programa, e em seguida, ajustar os tamanhos e posições.

No primeiro encontro, a sintaxe para definir a posição e o tamanho de um elemento filho dentro de um AbsoluteLayout pode parecer um pouco estranho. Se a view é um objeto do tipo View e o retângulo é um valor do tipo Rectangle, aqui está um exemplo de obter a localização e tamanho do retângulo:

AbsoluteLayout.SetLayoutBounds(view, rect);

Essa não é uma instancia do AbsoluteLayout na chamada do método SetLayoutBounds, e sim um método da classe AbsoluteLayout. Você chamar esse método antes ou depois de adicionar o elemento filho a coleção do AbsoluteLayout, pois é um método estático. Uma instancia particular de um AbsoluteLayout não está envolvido com todos os elementos no método SetLayoutBounds.

Vamos observar outro código que faz uso desse misterioso método SetLayoutBounds e examinar como ele funciona.

O construtor da página do programa ChessBoardDynamic usa o simples método Add sem posicionar ou dimensionar para adicionar os 32 elementos BoxView no AbsoluteLayout em um loop. Para proporcionar uma margem ao redor do tabuleiro, o AbsoluteLayout é um elemento filho de um ContentView e o espaçamento é configurado na página. O ContentView possui um evento SizeChanged para posicionar e dimensionar o AbsoluteLayout baseado no tamanho do seu conteúdo.

public class ChessboardDynamicPage : ContentPage

{

AbsoluteLayout absoluteLayout;

public ChessboardDynamicPage()

{

absoluteLayout = new AbsoluteLayout

{

BackgroundColor = Color.FromRgb(240, 220, 130), HorizontalOptions = LayoutOptions.Center, VerticalOptions = LayoutOptions.Center

};

for (int i = 0; i &lt; 32; i++)

{

BoxView boxView = new BoxView

{

Color = Color.FromRgb(0, 64, 0)

};

absoluteLayout.Children.Add(boxView);

}

ContentView contentView = new ContentView

{

Content = absoluteLayout

};

contentView.SizeChanged += OnContentViewSizeChanged;

this.Padding = new Thickness(5, Device.OnPlatform(25, 5, 5), 5, 5);

this.Content = contentView;

}

void OnContentViewSizeChanged(object sender, EventArgs args)

{

ContentView contentView = (ContentView)sender;

double squareSize = Math.Min(contentView.Width, contentView.Height) / 8;

int index = 0;

for (int row = 0; row &lt; 8; row++)

{

for (int col = 0; col &lt; 8; col++)

{

// Skip every other square. if (((row ^ col) &amp; 1) == 0)

continue;

View view = absoluteLayout.Children[index]; Rectangle rect = new Rectangle(col * squareSize,

row * squareSize,

squareSize, squareSize);

AbsoluteLayout.SetLayoutBounds(view, rect);

index++;

}

}

}

}

O evento SizeChanged possui muito da mesma lógica que o construtor definido com o atributo Fixed, exceto se o elemento BoxView já está na coleção de elementos filhos do AbsoluteLayout. Tudo que é necessário é a posição e o tamanho de cada BoxView quando o tamanho do conteúdo muda. O loop conclui com a chamada do método estático SetLayoutBounds para cada BoxView que é calculado.

Agora o tabuleiro de xadrez é dimensionando para preencher a tela com uma pequena margem:

Obviamente, o método SetLayoutBounds funciona, mas como? O que isso faz? E como é que ele consegue fazer o que ele faz, sem fazer referência a um objeto AbsoluteLayout?

A chamada do método SetLayoutBounds no evento SizeChanged é esse:

AbsoluteLayout.SetLayoutBounds(view, rect);

Essa chamada é exatamente equivalente a chamada a seguir:

view.SetValue(AbsoluteLayout.LayoutBoundsProperty, rect);

Essa é a chamada SetValue no elemento view. Essas duas chamadas de métodos são exatamente equivalentes pois o segundo é como o AbsoluteLayout define internamente o método estático SetLayoutBounds.

Você deve se lembrar que o SetValue e GetValue são definidos por BindableObject e é usado para implementar propriedades bindable. A julgar unicamente a partir do nome, AbsoluteLayout.LayoutBoundsProperty certamente parece ser um objeto BindableProperty, e realmente é. No entanto, é um tipo muito especial de propriedade bindable chamado como propriedade bindable anexado.

Propriedades bindable normais podem ser definidos apenas em instâncias da classe que define a propriedade ou sobre as instancias de uma classe derivada. Propriedades bindable anexadas quebram essa regra: São definidas por uma classe, neste caso AbsoluteLayout, mas definido como em outro objeto, neste caso como um elemento filho de AbsoluteLayout. A propriedade as vezes é anexado ao elemento filho, por isso o seu nome.

O elemento filho do AbsoluteLayout é ignorado com a finalidade de passar a propriedade bindable anexada para o método SetValue, e esse elemento filho não faz uso desse valor na sua lógica interna. O método SetValue do elemento simplesmente salva o valor de Rectangle em um dicionário mantido pelo BindableObject com o elemento, anexando o valor para ser usado possivelmente em algum momento pelo objeto pai no AbsoluteLayout.

Quando o AbsoluteLayout está colocando seus elementos filhos, pode interrogar o valor dessa propriedade em cada elemento chamando o método estático GetLayoutBounds, o qual chama o GetValue com a propriedade bindable. A chamada para GetValue busca o valor do Rectangle no dicionário armazenado no elemento.

Você pode ser perguntar: Por que tal processo é necessário para definir o posicionamento e o dimensionamento em um elemento filho do AbsoluteLayout? Não teria sido mais fácil para a View definir simplesmente o valor de X e Y?

Talvez, mas essas propriedades seriam adequadas apenas para o AbsoluteLayout. Ao usar um Grid, um aplicativo precisa especificar os valores das linhas e colunas nos elementos filhos da Grid e quando usar uma classe de layout de sua própria invenção, talvez algumas outras propriedades são necessárias. Propriedades bindable anexadas podem lidar com todos esses casos e muito mais.

Propriedades bindable anexadas são um mecanismo de uso geral que permite que as propriedades definidas por uma classe sejam armazenadas em instancias de outras classes. Você pode definir suas próprias propriedades bindable anexadas utilizando métodos de criação estáticos de um objeto BindableObject nomeado CreateAttached e CreateAttachedReadOnly.

Propriedades anexadas são utilizadas principalmente em classes de layout. Como você verá, a um Grid define propriedades bindable anexadas para especificar as linhas e colunas de cada elemento, e o RelativeLayout também define propriedades bindable anexadas.

Anteriormente você viu métodos Add definidos pela coleção de elementos filhos de um AbsoluteLayout. Este são, na verdade, implementados usando essas propriedades anexadas. A chamada:

absoluteLayout.Children.Add(view, rect);

é implementada como:

AbsoluteLayout.SetLayoutBounds(view, rect);

absoluteLayout.Children.Add(view);

A chamada ao método Add com apenas um argumento do tipo Point define a posição do elemento filho:

absoluteLayout.Children.Add(view, new Point(x, y));

Isso é implementado com o mesmo método estático SetLayoutBounds mas usando uma constante especial para a view:

AbsoluteLayout.SetLayoutBounds(view,

new Rectangle(x, y, AbsoluteLayout.AutoSize, AbsoluteLayout.AutoSize));

absoluteLayout.Children.Add(view);

Você pose usar a constante AbsoluteLayout.AutoSize em seu próprio código.