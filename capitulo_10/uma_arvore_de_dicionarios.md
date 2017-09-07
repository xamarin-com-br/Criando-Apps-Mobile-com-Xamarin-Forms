## Uma árvore de dicionários {#uma-rvore-de-dicion-rios}

A classe ResourceDictionary impõe as mesmas regras que outros dicionários: todos os itens no dicionário deve ter chaves, mas chaves duplicadas não são permitidos.

No entanto, porque cada instância de VisualElement potencialmente tem o seu próprio dicionário de recursos, sua página pode conter vários dicionários, e você pode usar as mesmas chaves em diferentes dicionários apenas contanto que todas as chaves dentro de cada dicionário são únicas. É concebível que cada elemento visual na árvore visual pode ter seu próprio dicionário, mas realmente só faz sentido um dicionário de recursos se aplicar a vários elementos, então dicionários de recursos só são encontrados geralmente definido em Layout ou objetos Page.

Você pode construir uma árvore de dicionários com chaves de dicionário que efetivamente sobrescrevem as chaves em outros dicionários. Isto é demonstrado no projeto ResourceTrees. O arquivo XAML para a classe ResourceTreesPage mostra um dicionário de Resources para a Content Page que define recursos com chaves de horzOptions , vertOptions e textColor . O item textColor demonstra como usar OnPlatform em um ResourceDictionary.

Um segundo dicionário de Resources é anexado a um StackLayout interior para recursos nomeados textColor e FontSize:

O dicionário de Resources no interior do StackLayout aplica-se apenas aos itens de dentro dessa StackLayput, que são os itens nesta captura de tela:

Veja como funciona:

Quando o analisador XAML encontra um StaticResource em um atributo de um elemento visual, começa a procurar a chave dicionário. A principio no ResourceDictionary para esse elemento visual, e se a chave não for encontrada, ele procura a chave no ResourceDictionary pai e acima e acima através da árvore visual até que ele atinja a ResourceDictionary na página.

Mas algo está faltando aqui! Onde estão os itens de dicionário borderWidth e FontSize? Eles não parecem estar definidos no dicionário de recursos da página!

Esses itens estão em outro lugar. A classe Application também define uma propriedade Resources do tipo ResourceDictionary. Isso é útil para a definição de recursos que se aplicam a todo o aplicativo e não apenas a uma página ou layout específico. Quando o analisador XAML pesquisa a árvore visual para uma correspondência chave de recurso, e a chave não é encontrada na ResourceDictionary para a página, ele finalmente verifica o ResourceDictionary definido pela classe Application. Só se ele não for encontrado há uma XamlParseException lançada pela StaticResourcechave.

O modelo de solução padrão gera um Xamarin.Forms gera uma classe App que deriva de Application e, assim, herda a propriedade Resources. Você pode adicionar artigos a este dicionário de duas maneiras:

Uma abordagem é para adicionar os itens em código no App construtor. Certifique-se de fazer isso antes de instanciar a classe ContentPage principal:

No entanto, a classe App também pode ter um arquivo XAML próprio, e os recursos de todo o aplicativo pode ser definido na coleção Resources no arquivo XAML.

Para fazer isso, você vai querer apagar o arquivo App.cs criado pelo modelo de solução Xamarin.Forms. Não há nenhum item de modelo para uma classe App, então você precisa fingir. Adicionar uma nova página de classe XAML - Forms Xaml page no Visual Studio ou Forms ContentPage Xaml em Xamarin Studio - ao projeto. Nomeie o App . E imediatamente, antes que esqueça - vá para o arquivo App.xaml e altere as tags raiz para Application , e vá para o arquivo App.xaml.cs e altere a classe base para Application.

Agora você tem uma classe App que deriva de Application e tem seu próprio arquivo XAML. No arquivo App.xaml que você pode, em seguida, instanciar um ResourceDictionary dentro de tags de propriedade de elementos Application.Resources e adicionar itens a ela:

O construtor no arquivo code-behind precisa chamar o InitializeComponent para analisar o arquivo App.xaml em tempo de execução e adicionar os itens ao dicionário. Isto deve ser feito antes do trabalho normal de instância da classe ResourceTreesPage e defini-lo como propriedade MainPage:

Adicionar eventos de ciclo de vida é opcional.

Certifique-se de chamar InitializeComponent antes de instanciar a classe de página. O construtor da classe de página chama sua própria InitializeComponent para analisar o arquivo XAML para a página, e as extensões de marcação StaticResource precisam de acesso às coleções de Resources na classe App.

Cada dicionário de Resource tem um escopo específico: Para o dicionário de Resources na classe App, esse escopo é o aplicativo inteiro. Um dicionário de Resources na ContentPage aplica-se à classe página inteira.

Como você verá no Capítulo 12, os itens mais importantes em um dicionário de Resources são geralmente objetos do tipo Style. Em geral, você terá Styes para todo o aplicativo, objetos Style para a página, e objetos Style associados com partes menores da árvore visual.