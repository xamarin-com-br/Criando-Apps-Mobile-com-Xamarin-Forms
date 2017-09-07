## Pré-processamento no compartilhamento do Projeto {#pr-processamento-no-compartilhamento-do-projeto}

Como você aprendeu no Capítulo 2, &quot;Anatomia de um aplicativo&quot;, você pode usar tanto o compartilhamento do projeto ou uma Biblioteca de Classes Portátil para o código que é comum a todas as três plataformas. O compartilhamento do projeto contém arquivos de código que são compartilhados entre os projetos da plataforma, enquanto as classes portáteis incluem o código comum em uma biblioteca que só é acessível através de tipos públicos.

Acessar APIs da plataforma a partir de um projeto com recursos compartilhados é um pouco mais simples do que a partir de uma Biblioteca de Classes Portátil, porque envolve ferramentas de programação mais tradicionais, então vamos tentar essa abordagem em primeiro lugar. Você pode criar uma solução Xamarin.Forms com compartilhamento do projeto, utilizando o processo descrito no capítulo 2\. Você pode então adicionar uma classe ContentPage baseada em XAML para o local compartilhado da mesma maneira que você adiciona a uma classe portátil.

Aqui está o arquivo XAML para um projeto chamado DisplayPlatformInfoSap1:

O arquivo code-behind deve definir as propriedades de texto para modelLabel e versionLabel.

Arquivos de código compartilhado no projeto são extensões do código nas plataformas individuais. Isto significa que o código no compartilhamento pode fazer uso do C # pré-processamento, diretivas #if, #elif, #else, e #endif com compilação condicional definidas para as três plataformas, como demonstrado nos capítulos 2 e 4\. Estes símbolos são __IOS__ para iOS, __ANDROID__ para o Android, e WINDOWS_PHONE para Windows Phone.

As APIs envolvidas na obtenção de informações sobre o modelo e versão são, diferentes para as três plataformas:

*   Para iOS, use a classe UIDevice no namespace UIKit.
*   Para Android, use várias propriedades da classe Build no namespace Android.OS.
*   Para Windows Phone, use a classe devicestatus no namespace Microsoft.Phone.Info e a classe Ambiente no namespace System.

Aqui está o DisplayPlatformInfoSap1.xaml.cs arquivo code-behind que mostram como modelLabel e versionLabel são definidos com base nos símbolos de compilação condicional:

Observe que estas diretivas de preprocessor são usados para selecionar diferentes using diretivas, bem como para fazer chamadas para APIs específicas da plataforma. Em um programa tão simples como este, você poderia simplesmente incluir os namespaces com os nomes de classe, mas para longos blocos de código, você provavelmente vai querer aqueles que utilizam directivas.

E, claro, ele funciona:

A vantagem dessa abordagem é que você tem todo o código para as três plataformas em um único lugar. Mas a listagem de código que vamos enfrentar será muito feia, e isso remonta a uma época muito anterior em programação. Usando diretivas de pré-processamento pode não parecer tão ruim para chamadas curtas e menos freqüentes, como neste exemplo, mas em um programa maior que você vai precisar fazer malabarismos em blocos de código específico da plataforma e de código compartilhado, e a multidão de pré-processamento diretivas pode facilmente tornar-se confuso. Diretivas de pré-processador deve ser usado para pequenas correções e geralmente não como elementos estruturais no aplicativo.

Vamos tentar outra abordagem.