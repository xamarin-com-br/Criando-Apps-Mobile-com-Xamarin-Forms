## Dentro dos arquivos {#dentro-dos-arquivos}

Claramente, o programa criado pelo modelo Xamarin.Forms é muito simples, por isso esta é uma excelente oportunidade para examinar os arquivos de código gerado e descobrir suas inter-relações e como elas funcionam.

Vamos começar com o código que é responsável por definir o texto que você vê na tela. Esta é a classe App no projeto **Olá**. No projeto criado pelo Visual Studio, a classe App é definido no arquivo App.cs, mas no Xamarin Studio, o arquivo é Hello.cs.

Se o modelo de projeto não mudou muito desde que este capítulo foi escrito, provavelmente é algo como isto:

Observe que o espaço para nome é o mesmo que o nome do projeto. Esta classe **App** é definida como pública e deriva da classe Xamarin.Forms **Application**. O construtor realmente tem apenas uma responsabilidade: definir a propriedade **MainPage** da classe **Application** para um objeto do tipo de página.

O código que o modelo Xamarin.Forms mostra uma abordagem muito simples para definir esse construtor: A classe **ContentPage** deriva da página e é muito comum em uma única página Xamarin.Forms **Application**. (Você vai ver um monte de **ContentPage** ao longo deste livro.) Ele ocupa a maior parte da tela do telefone com a exceção da barra de status no topo da tela Android, os botões na parte inferior da tela Android, e a barra de status na parte superior da tela do Windows Phone. (Como você vai descobrir, a barra de status iOS é realmente parte do **ContentPage**.)

A classe **ContentPage** define uma propriedade chamada **Content** que você definiu para o conteúdo da página. Geralmente este conteúdo é um layout que por sua vez contém um monte de **views**, e, neste caso, ele é definido como um **StackLayout**, que organiza seus filhos em uma pilha.

Este **StackLayout** tem apenas um filho, que é um **Label**. A classe **Label** deriva da **View** e é usada em aplicações Xamarin.Forms para exibir até um parágrafo de texto. O **VerticalOptions** e a propriedade **xalign** são discutidos em mais detalhes mais adiante neste capítulo.

Para seus próprios aplicativos Xamarin.Forms uma única página, você geralmente irá definir sua própria classe que deriva de **ContentPage**. O construtor da classe **App**, define uma instância da classe que você define a sua propriedade **MainPage**. Você vai ver como isso funciona em breve.

Na solução **Hello**, você também verá um arquivo AssemblyInfo.cs para a criação do PCL e um arquivo de packages.config que contém os pacotes NuGet exigidos pelo programa. Na seção de referências em Olá na lista de solução, você verá as três bibliotecas este PCL requer:

*   .NET (displayed as .NET Portable Subset in Xamarin Studio)
*   Xamarin.Forms.Core
*   Xamarin.Forms.Xaml

Este é um projeto PCL que irá receber a maior parte de sua atenção como você está escrevendo uma aplicação Xamarin.Forms. Em algumas circunstâncias, o código neste projeto pode exigir alguma adaptação para as três plataformas diferentes, e você verá em breve como fazer isso. Você também pode incluir código específico da plataforma nos três projetos de aplicação.

Os três projetos de aplicação têm os seus próprios assets na forma de ícones e metadados, e você deve prestar atenção especial a esses assets, se você pretende trazer a aplicação para o mercado. Mas durante o tempo que você está aprendendo a desenvolver aplicações usando Xamarin.Forms, esses assets podem geralmente ser ignorado. Você provavelmente vai querer manter estes projetos de aplicação collapsed na solução, porque você não precisa se preocupar muito com seus conteúdos.

Mas você realmente deve saber o que está nesses projetos de aplicativos, por isso vamos dar uma olhada mais de perto.

Na seção de referências de cada projeto de aplicativo, você verá referências comuns para o projeto PCL (Olá, neste caso), bem como vários conjuntos .NET, os Xamarin.Forms assembles listados acima, e Xamarin.Forms assembles adicionais aplicáveis a cada plataforma:

*   Xamarin.Forms.Platform.Android
*   Xamarin.Forms.Platform.iOS
*   Xamarin.Forms.Platform.WP8

Cada uma destas três bibliotecas define um método estático Forms.Init no namespace Xamarin.Forms que inicializa o sistema Xamarin.Forms para essa plataforma particular. O código de inicialização em cada plataforma deve fazer uma chamada para esse método.

Você também acabou de ver que o projeto PCL deriva uma classe pública denominada App que deriva da Aplicação. O código de inicialização em cada plataforma também deve instanciar essa classe App.

Se você estiver familiarizado com iOS, Android, ou o desenvolvimento do Windows Phone, você pode estar curioso para ver como o código de inicialização da plataforma lida com essa forma de trabalho.