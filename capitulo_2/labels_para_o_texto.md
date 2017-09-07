## Labels para o texto {#labels-para-o-texto}

Vamos criar uma nova solução Xamarin.Forms PCL, Greetings, usando o mesmo processo descrito acima para criar a solução Hello. Esta nova solução será estruturada mais como um programa Xamarin.Forms, o que significa que ele irá definir uma nova classe que deriva de ContentPage. Na maioria das vezes neste livro, todas as classes e estruturas definidas por um programa terão seu próprio arquivo. Isto significa que um novo arquivo deve ser adicionado ao projeto Greetings:

No Visual Studio, você pode clicar com o botão direito no projeto Greetings no **Solution Explorer** e selecione **Add &gt; New Item** do menu. À esquerda da caixa de diálogo **Add New Item**, selecione **Visual C#** e código, e na área central, selecione **Forms ContentPage**. (Cuidado: Há também uma opção de formulários ContentView Não escolha esse!)

No Xamarin Studio, a partir do ícone da ferramenta no projeto Greetings, selecione **Add &gt; New File** no menu. Na esquerda da caixa de diálogo **New File**, selecione **Forms**, e na área central, selecione **Forms ContentPage**. (Cuidado: Há também Forms ContentView e formas ContentPage Xaml. Não escolher esses!)

Em ambos os casos, de o nome do novo arquivo de GreetingsPage.cs.

O arquivo GreetingsPage.cs será inicializado com algum código para uma classe chamada GreetingsPage que deriva de ContentPage. Porque ContentPage está no namespace Xamarin.Forms, usando uma diretiva que inclui namespace. A classe é definida como pública, mas ela não precisa ser, porque não vai ser acessado diretamente de fora do projeto Greetings.

Vamos apagar todo o código no construtor GreetingsPage e a maioria das diretivas usadas, assim que o arquivo se parece com isto:

No construtor da classe GreetingsPage, deve-se instanciar um Label view, defina sua propriedade de texto, e definir essa instância Label para a propriedade de conteúdo que GreetingsPage herda ContentPage:

Agora altere a classe App em App.cs para definir a propriedade MainPage a uma instância desta classe GreetingsPage:

É fácil esquecer este passo, e você vai ficar perplexo que seu programa parece ignorar completamente a sua classe de página e ainda diz &quot;Welcome to Xamarin Forms!&quot;

É na classe GreetingsPage (e outros como ela), onde você estará gastando mais de seu tempo na programação do Xamarin.Form. Esta classe pode conter o único código do aplicativo. Claro, você pode adicionar classes adicionais para o projeto, se você precisar.

A classe que deriva de ContentPage terá um nome que é o mesmo que o aplicativo, mas com Página anexada. Essa convenção de nomenclatura deve ajudá-lo a identificar as listagens de código neste livro a partir de apenas o nome da classe ou construtor sem ver o arquivo inteiro. Na maioria dos casos, os trechos de código nas páginas deste livro não incluem as diretivas using ou a definição namespace.

Muitos programadores Xamarin.Forms preferem usar o estilo de criação de objetos e inicialização de propriedade C# 3.0 em seus construtores da página. Seguindo o construtor Label, um par de chaves delimitam uma ou mais definições de propriedades separadas por vírgula. Está aqui uma alternativa (mas funcionalmente equivalente) definição GreetingsPage:

Este estilo permite a ocorrência de Label a ser definida para a propriedade de conteúdo diretamente, de modo que a etiqueta não exige um nome, assim:

Para mais layouts de páginas complexos, este estilo de instanciação e inicialização fornece um visual melhor analógico da organização de layouts e views na página. No entanto, nem sempre é tão simples como este exemplo pode indicar se você precisa chamar métodos nesses objetos ou manipuladores de eventos definidos.

Independentemente da forma como você faz isso, se você pode compilar e executar o programa nas três formas de plataformas de cada emulador ou um dispositivo com sucesso, aqui está o que você vai ver:

A versão deste programa Greetings é definitivamente o iPhone: A partir do iOS 7, um aplicativo uma única página partes da tela com a barra de status no topo. Qualquer coisa exibida na parte superior de sua página vai ocupar o mesmo espaço que a barra de status, a menos que a aplicação do compense para ele.

Este problema desaparece em aplicações multipage-navigation discutidos mais adiante neste livro, mas até esse momento, aqui estão quatro maneiras (ou cinco maneiras se você estiver usando um SAP) para resolver este problema imediatamente.