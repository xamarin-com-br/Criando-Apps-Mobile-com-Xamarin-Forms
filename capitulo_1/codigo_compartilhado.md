## Código Compartilhado {#c-digo-compartilhado}

A vantagem da segmentação em várias plataformas com uma única linguagem de programação vem da capacidade de compartilhar código entre as aplicações.

Antes de código pode ser compartilhado, um aplicativo deve ser estruturado para esse efeito. Particularmente desde o amplo uso de interfaces gráficas de usuário, os programadores têm entendido a importância de separar o código do aplicativo em camadas funcionais. Talvez a divisão mais útil é entre o código de interface do usuário e os modelos de dados subjacentes e algoritmos. O MVC popular (MVC) arquitetura de aplicação apro- formaliza esta separação código em um modelo (os dados subjacentes), o Vista (a representação visual dos dados), eo Controller (que lida com a entrada do usuário).

MVC originou na década de 1980\. Mais recentemente, a arquitetura (Model-View-ViewModel) MVVM tem efetivamente modernizado MVC com base em interfaces gráficas modernas. MVVM separa código no Modelo (a compreensão de dados mentir), a View (interface do usuário, incluindo recursos visuais e de entrada), e o ViewModel (que dados idades-homem passando entre o modelo e a vista).

Quando um programador desenvolve uma aplicação que tem como alvo várias plataformas móveis, a arquitetura MVVM ajuda a orientar o desenvolvedor para separar código no Ver-o código específico da plataforma que requer a interação com as APIs-e plataforma do modelo independente de plataforma e Ver- Modelo.

Muitas vezes este código independente de plataforma precisa acessar arquivos ou a rede ou coleções de uso ou threading. Normalmente, estes postos de trabalho seriam considerados parte de uma API do sistema operacional, mas eles também são postos de trabalho que podem fazer uso da biblioteca de classes .NET Framework, e se a biblioteca de classes está disponível em cada plataforma, é efetivamente independente de plataforma.

A parte do aplicativo que é independente de plataforma pode então ser isolado e-no contexto do Visual Studio ou Xamarin Studio-colocado em um projeto separado. Isso pode ser um projeto de recurso compartilhado (SAP) -que consiste simplesmente em arquivos de código e outros ativos acessível a partir de outros projetos, ou uma Biblioteca de Classes Portátil (PCL), que abrange todo o código comum em uma biblioteca de vínculo dinâmico (DLL) que podem então ser referenciado a partir de outros projetos.

Independentemente do método utilizado, este código comum tem acesso à biblioteca de classes .NET Framework, para que ele possa executar arquivo de I / O, lidar com a globalização, os serviços Web Access, decompor XML, e assim por diante.

Isso significa que você pode criar uma única solução Visual Studio que contém quatro projetos C # para alvo conseguir os três principais plataformas móveis (todos com acesso a um SAP comum ou projeto PCL), ou você pode usar Xamarin Studio para direcionar dispositivos iPhone e Android .

O diagrama a seguir ilustra as inter-relações entre os projetos do Visual Studio ou Xamarin Studio, as bibliotecas Xamarin, e as APIs da plataforma:

![An illustration showing the interrelationships between the Visual Studio or Xamarin Studio projects, the Xamarin libraries, and the platform APIs.](../assets/an_illustration_showing_the_interre.png)

As caixas da segunda fila são as aplicações reais específicos da plataforma. Esses aplicativos fazer chamadas para o projeto comum e também (com o iPhone e Android) as bibliotecas Xamarin que implementam as APIs de plataforma nativa.

Mas o diagrama não é bastante completo: ele não mostra a SAP ou PCL fazer chamadas para a biblioteca de classes .NET Framework. O PCL tem acesso à sua própria versão do .NET, enquanto a SAP utiliza a versão do .NET incorporados em cada plataforma particular.

Neste diagrama, as bibliotecas Xamarin.iOS e Xamarin.Android parecem ser substanciais, e enquanto eles são certamente importantes, eles são na sua maioria ligações apenas de linguagem e não adicionar significativamente qualquer sobrecarga de chamadas de API.

Quando o iPhone app é construído, o # compilador Xamarin C gera C # Intermediate Language (IL), como de costume, mas, em seguida, faz uso do compilador da Apple no Mac para gerar código nativo máquina de iPhone como o compilador Objective-C. As chamadas do aplicativo às APIs do iPhone são o mesmo como se o aplicativo foi escrito em Objective-C.

Para o aplicativo para Android, o # compilador Xamarin C gera IL, que é executado em uma versão do Mono no dispositivo ao lado do motor de Java, mas as chamadas de API do aplicativo são praticamente o mesmo como se o app foram escritas em Java.

Para aplicações móveis que têm necessidades muito específicas da plataforma, mas também um pedaço potencialmente compartilhável de código independente de plataforma, Xamarin.iOS e Xamarin.Android fornecer excelentes soluções. Você tem acesso a toda a API da plataforma, com todo o poder (e responsabilidade) isso implica.

Mas para aplicações que não pode precisar bastante tanta especificidade plataforma, existe agora uma alternativa que irá simplificar a sua vida ainda mais.