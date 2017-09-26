## StaticResource para a maioria dos propósitos {#staticresource-para-a-maioria-dos-fins}

Suponha que você tenha definido três botões em um **StackLayout **:![](/assets/10-20-stacklayou)![](/assets/10-20-stacklayout1)![](/assets/10-20-stacklayout3)Claro, este é um código um tanto irrealista. Não há eventos **Clicked **setados para estes botões, e a **BorderWidth **não tem efeito nos dispositivos Android porque o fundo botão e cor da borda têm seus valores padrão. Veja a aparência dos botões:![](/assets/10-21-telas)

Para além do texto, todas as três teclas têm as mesmas propriedades definidas para os mesmos valores. Maracações repetitivas como estas tendem a levar os programadores para o caminho errado. É uma afronta aos olhos e difícil de manter e mudar.

Eventualmente, você vai ver como usar estilos para realmente diminuir a marcação repetitiva. Por enquanto, de qualquer forma, o objetivo não é fazer a marcação mais curta, mas consolidar os valores em um só lugar para que se você quiser mudar a propriedade **TextColor **de Vermelho para Azul, você pode fazê-lo com uma edição o invés de três.

Obviamente, você pode usar **x:Static **para este trabalho, definindo os valores no código. Mas vamos fazer tudo em XAML, armazenando os valores em um dicionário de recursos. Cada classe que deriva de **VisualElement **tem uma propriedade Resources do tipo **ResourceDictionary**. Recursos que são usados ao longo de uma página são habitualmente armazenados na coleção Resources da Página de conteúdo.

O primeiro passo consiste em expressar a propriedade **Resources **de Página de conteúdo como um elemento de propriedade:![](/assets/10-22-resource)Se você também está definindo uma propriedade **Padding **na página usando tags de propriedade elementos, a ordem não importa.

Para efeitos de desempenho, a propriedade **Resources **é **nula **por padrão, então você precisa explicitamente instanciar o **ResourceDictionary **:![](/assets/10-23-instanciaderesource)Entre as tags **ResourceDictionary**, você define um ou mais objetos ou valores. Cada item no dicionário deve ser identificado com uma chave de dicionário que você especificar com o atributo XAML **x: Key**. Por exemplo, aqui está a sintaxe para a inclusão de um valor **LayoutOptions **no dicionário com uma chave descritiva que indica que este valor é definido para a setar opções horizontais:![](/assets/10-24-layot)Porque este é um valor **LayoutOptions**, o analisador XAML acessa a classe **LayoutOptionsConverter **\(que é private para Xamarin.Forms\) para converter o conteúdo das tags, que é o texto "centro".

Uma segunda maneira de armazenar um valor **LayoutOptions **no dicionário é deixar o analisador XAML instanciar a estrutura e e setar as propriedades **LayoutOptions **de atributos especificados:![](/assets/10-25-layoutoption)A propriedade **BorderWidth **é do tipo double, então o elemento tipo de dado **x:Double** definido na especificação XAML 2009 é ideal:![](/assets/10-26-double)Você pode armazenar um valor **Color **no dicionário de recursos com uma representação de texto da cor como conteúdo. O analisador XAML usa o **ColorTypeConverter **normal para a conversão de texto:![](/assets/10-26-color)Não é possível inicializar um valor de cor, definindo suas propriedades R, G e B porque essas são apenas get-only.

Mas você pode invocar um construtor de cores usando **x: Arguments** ou um dos métodos de fábrica de cores usando **x:FactoryMethod **e **x:Arguments**.![](/assets/10-27-xargments)Observe os atributos **x:Key **e **x:FactoryMethod**.

As propriedades **BackgroundColor **e **BorderColor **dos três botões acima são definidas para valores da classe **OnPlatform**. Felizmente você pode colocar objetos **OnPlatform **diretamente no dicionário:![](/assets/10-28-onplataform)Um item de dicionário para a propriedade **FontSize **é um pouco problemático. A propriedade **FontSize** é do tipo **Double**, então se você está armazenando um valor numérico real no dicionário, isso não é problema. Mas não é possível armazenar a palavra "**grande**" no dicionário como se fosse um double. Somente quando uma string "**grande**" é definida como um atributo **FontSize **é que o analisador XAML usa o **FontSizeConverter**. Por essa razão, você vai precisar para armazenar o item **FontSize **como uma string:![](/assets/10-28-string)Aqui está o dicionário completo neste momento:![](/assets/10-29-dicionariocompleto)

Esta é muitas vezes referida como uma seção de recursos para a página. 

Na programação da vida real, quase todos os arquivos XAML começam com uma seção de recursos. 

Você pode fazer referência a itens no dicionário usando a extensão de marcação **StaticResource**, que é suportado pela **StaticResourceExtension**, uma classe privada para Xamarin.Forms. **StaticResourceExtension **define uma propriedade chamada **key **que definiu para o dicionário de chaves. Você pode usar uma **StaticResourceExtension **como um elemento dentro de tags de propriedade de elementos, ou você pode usar **StaticResourceExtension** ou **StaticResource **entre chaves. Se você está usando a sintaxe chave, você pode deixar de fora a **Key **e sinais iguais porque a **Key **é o conteúdo da propriedade de **StaticResourceExtension**.

O seguinte arquivo XAML completo no projeto **ResourceSharing **ilustra três dessas opções:

![](/assets/10-30resourcesharing)![](/assets/10-30-resourcesharing1)![](/assets/10-30-resourcesharing3)![](/assets/10-30-resourcesharing4)![](/assets/10-30-resourcesharing5)A sintaxe mais simples do terceiro botão é a mais comum e, na verdade, que a sintaxe é tão onipresente que muitos desenvolvedores XAML de longa data podem estar totalmente familiarizados com as outras variações.

Objetos e valores no dicionário são compartilhados entre todas as referências StaticResource. Não é tão claro no exemplo anterior, mas é algo a ter em mente. Por exemplo, suponha que você armazene um objeto Button no dicionário de recursos:

Você pode certamente usar esse objeto Button em sua página, adicionando-o às coleções Children de um StackLayout com o elemento de sintaxe StaticResourceExtension:

No entanto, você não pode usar esse mesmo item de dicionário na esperança de colocar outra cópia no StackLayout :

Isso não vai funcionar. Ambos os elementos fazem referência ao mesmo objeto Button, e um elemento visual específicamente podem estar em apenas um local particular na tela. Ele não pode estar em vários locais.

Por esta razão, elementos visuais não são normalmente armazenados num dicionário de recursos. Se você precisar de multiplos elementos em sua página que têm na sua maioria as mesmas propriedades, você vai querer usar um Style, o que é explorada no Capítulo 12.

