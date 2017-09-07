## Pixels, points, dps, DIPs, e DIUs {#pixels-points-dps-dips-e-dius}

As soluções com pixels começaram com sistemas operacionais para computadores desktop, e estas soluções foram, em seguida, adaptadas para dispositivos móveis. Por esta razão, é esclarecedor para começar essa exploração com o desktop.

Monitores de desktop têm uma ampla gama de dimensões em pixels, da quase obsoleta 640 × 480, no máximo, para os milhares. A relação de aspecto de 4:3 já foi padrão para telas de computador e para cinema e televisão, a relação de aspecto de alta definição de 16: 9 (ou semelhante 16:10) é agora mais comum.

Monitor de desktop também têm uma dimensão física, geralmente é medido ao longo da diagonal da tela em polegadas ou centímetros. A dimensão do pixel combinada com a dimensão física permite calcular resolução ou pixel densidade de exibição do vídeo em pontos por polegada (DPI), às vezes também referidos como pixels por polegada (PPI). A resolução da tela também pode ser medida como um passo do ponto, que é a distância entre os centros dos pixels adjacentes, normalmente medido em milímetros.

Por exemplo, você pode usar o teorema de Pitágoras para calcular um display antigo 800 × 600 tem um comprimento diagonal que seria acomodar 1.000 pixels na horizontal ou na vertical. Se este monitor tem uma diagonal de 13 polegadas, que é uma densidade de pixels de 77 DPI, ou um dot pitch de 0,33 milímetros. No entanto, a tela de 13 polegadas em um laptop moderno pode ter dimensões de pixel de 2560 × 1600, que é cerca de 230 DPI, ou um passo do ponto de cerca de 0,11 milímetros. Um objeto quadrado de 100 pixel na tela é um terço do tamanho do mesmo objeto na tela mais antiga.

Por esta razão, os sistemas de computação de desktop permitem que os programadores trabalhem com a exibição de vídeo em alguma forma de unidades independentes de dispositivo, em vez de pixels como a Apple e a Microsoft conceberam. A maior parte das dimensões que um programador encontra e especifica estão nessas unidades independentes de dispositivo. É de responsabilidade do sistema operacional converter estas unidades e pixels.

No mundo da Apple, monitor de desktop foram tradicionalmente assumindo uma resolução de 72 unidades por polegada. Este número vem da tipografia, onde muitas medições são expressas em unidades de pontos. Em tipografia clássica, existem cerca de 72 pontos por polegada, mas em tipografia digital, o ponto foi padronizado para exatamente 1/72 de uma polegada. Ao trabalhar com pontos em vez de pixels, um programador tem um sentido intuitivo da relação entre os tamanhos numéricos da área que ocupam os objetos visuais na tela.

No mundo Windows, uma técnica similar foi desenvolvida, chamado pixels independentes de dispositivo (DIPs) ou unidades independentes de dispositivo (UDE). Para um programador do Windows, na área de trabalho é assumido uma resolução de 96 DIUs, que é exatamente um terço superior ao 72 DPI, embora possa ser ajustada.

Os dispositivos móveis, no entanto, têm regras um pouco diferentes: As densidades de pixels obtidos em telefones modernos, são tipicamente muito maiores do que em desktops. Esta maior densidade de pixels permite que o texto e outros objetos visuais encolha muito mais em tamanho.

Telefones fica mais perto do rosto do usuário do que é uma tela de desktop ou laptop. Esta diferença também implica que os objetos visuais no telefone podem ser menores do que objetos nas telas de desktop ou laptop. Porque as dimensões físicas do telefone são muito menores do que o desktop exibe, encolhendo para baixo objetos visuais é muito desejável, pois permite caber muito mais na tela.

A Apple continua referindo-se às unidades independentes de dispositivo no iPhone como pontos. Até recentemente, todos os Apple com alta densidade exibem (que a Apple se refere pelo nome Retina) tem uma conversão de dois pixels para o ponto. Isto era verdade para o MacBook Pro iPad e iPhone. A exceção recente é o iPhone 6 Plus, que tem três pixels para o ponto.

Por exemplo, o 640 × 960 pixels dimensão da tela de 3,5 polegadas do iPhone 4 tem uma densidade de pixels real de cerca de 320 DPI. Há dois pixels por ponto, então um programa aplicativo em execução no iPhone 4, a tela parece ter uma dimensão de 320 × 480 pontos. O iPhone 3, na verdade, tinha uma dimensão de pixel de 320 × 480, e pontos igual ou pixels, então um programa, o video do iPhone 3 e do iPhone 4 parecem ser do mesmo tamanho. Apesar dos mesmos tamanhos percebidos, gráficos e texto são exibidos em maior resolução sobre o iPhone 4 do que o iPhone 3\.

Para o iPhone 3 e iPhone 4, a relação entre o tamanho da tela e dimensões de ponto em pixels tem um fator de conversão de 160 pontos por polegada em vez do padrão de desktop de 72\.

O iPhone 5 tem uma tela de 4 polegadas, mas a dimensão dos pixel é 640 × 1136\. A densidade de pixels é quase o mesmo que o iPhone 4\. Para um programa, esta tela tem um tamanho de 320 × 768 pontos.

O iPhone 6 tem uma tela de 4,7 polegadas e uma dimensão de pixel de 750 × 1334\. A densidade de pixels também é de cerca de 320 DPI. Existem dois pixels para o ponto, deste modo que um programa, a tela parece ter um tamanho de ponto de 375 × 667\.

No entanto, o iPhone 6 Plus tem uma tela de 5,5 polegadas e uma dimensão de pixel de 1080 × 1920, que é uma densidade de pixels de 400 DPI. Esta maior densidade de pixels implica mais pixels ao ponto, e para o iPhone 6 Além disso, a Apple estabeleceu o ponto igual a três pixels. Que normalmente implica um tamanho de tela percebida de 360 × 640 pontos, mas a um programa, a tela do iPhone 6 Plus tem um tamanho de ponto de 414 × 736, deste modo a resolução percebida é de cerca de 150 pontos por polegada.

Esta informação está resumida na tabela a seguir:

| **Modelo** | **iPhone 2.3** | **iPhone 4** | **iPhone 5** | **iPhone 6** | **iPhone 6 Plus** |
| --- | --- | --- | --- | --- | --- |
| **Tamanho Pixel** | 320 X 480 | 640 X 960 | 640 X 1136 | 750 × 1334 | 1080 × 1920 |
| **Tela Diagonal** | 3.5 in. | 3.5 in. | 4 in. | 4.7 in. | 5.5 in. |
| **Densidade Pixel** | 165 DPI | 330 DPI | 326 DPI | 326 DPI | 401 DPI |
| **Pixels por Ponto** | 1 | 2 | 2 | 2 | 3 |
| **Tamanho do Pont** | 320 × 480 | 320 × 480 | 320 × 568 | 375 × 667 | 414 × 736 |
| **Pontos por Polegada** | 165 | 165 | 163 | 163 | 154 |

Android faz algo bastante semelhante: dispositivos Android têm uma grande variedade de tamanhos e dimensões de pixel, um programador Android geralmente trabalha com unidades independentes densidade pixels (dps). A relação entre pixels e dps é ajustado 160 dps por polegada, o que significa que a Apple e dispositivos Android são muito semelhantes.

Microsoft teve uma abordagem diferente com o Windows Phone. Windows Phone 7 dispositivos

A Apple continua a referir-se às unidades independentes de dispositivo no iPhone como pontos. Até recentemente, todos tem uma dimensão de pixel uniforme de ambos os 320 × 480, mas os dispositivos que utilizam este tamanho de tela eram muito raros e podem ser ignoradas para esta discussão, ou 480 × 800, que é muitas vezes referida como WVGA (Wide Video Graphics Array). 7 programas Windows Phone trabalhar com esta exposição em unidades de pixels. Se você assumir um tamanho de tela média de 4 polegadas para um dispositivo de 480 × 800 Windows Phone 7, isto significa que o Windows Phone é implicitamente assumindo uma densidade de pixels de cerca de 240 DPI. Isso é 1,5 vezes o pixel den-sidade assumido de aparelhos iPhone e Android.

Com o Windows Phone 8, vários tamanhos de tela maior é permitido: 768 × 1280 (WXGA ), 720 × 1280 (referido usando de alta definição jargão televisão como 720p), e 1080 × 1920 (chamado 1080p) .

Isto está resumido na tabela seguinte:

| **Tipo de Tela** | **WVGA** | **WXGA** | **720p** | **1080p** |
| --- | --- | --- | --- | --- |
| **Tamanho Pixel** | 480 × 800 | 768 × 1280 | 720 × 1280 | 1080 × 1920 |
| **Fator escala** | 1 | 1.6 | 1.5 | 2.25 |
| **Tamanho em DIUs** | 480 × 800 | 480 × 800 | 480 × 853 | 480 × 853 |

Xamarin.Forms tem uma filosofia de utilizar de convenção das plataformas subjacentes, tanto quanto possível. De acordo com esta filosofia, um programador Xamarin.Forms trabalha com tamanhos definidos por cada plataforma particular. Todos os tamanhos que o programador encontra através da API Xamarin.Forms estão nessas, unidades independentes de dispositivo específicos da plataforma.

Programadores Xamarin.Forms podem tratar o visor do telefone de uma forma independente do dispositivo, mas um pouco diferente para cada uma das três plataformas:

*   **iOS: assumir 160 unidades por polegada**
*   **Android: assumir 160 unidades por polegada**
*   **Windows Phone: assumir 240 unidades por polegada**

É desejável objetos visuais de tamanhos diferentes para as três plataformas, no Windows Phone deve ter cerca de 150 por cento maior do que as dimensões do iPhone e Android. Se você comparar o valor de 160 iOS com o valor da Apple, a área de trabalho de 72 unidades por polegada, e o valor do Windows Phone de 240 com o valor de desktop de 96 do Windows, você vai descobrir um pressuposto implícito de que os telefones são mantidos um pouco mais perto do que metade da distância entre os olhos do que é um monitor de mesa.

A classe VisualElement define duas propriedades, Width (largura) e Height (altura), que fornecem as dimensões prestados de pontos de vista, layouts e páginas nestas unidades independentes de dispositivo. No entanto, as configurações iniciais de largura e altura são valores &quot;fictícios&quot; de -1\. Os valores dessas propriedades se tornam válidos apenas quando o sistema de layout tem posicionado e dimensionado tudo na página. Além disso, mantenha em mente que o preenchimento padrão das opções horizontais ou opções verticais muitas vezes faz com que a view ocupe mais espaço do que seria de outra forma. Os valores de Width (largura) e Height (altura) refletem esse espaço extra. Os valores de largura e altura também incluir qualquer preenchimento que pode ser definido e são consistentes com a área colorida por BackgroundColor propriedade da view.

VisualElement define um evento chamado SizeChanged que é acionado sempre que as propriedades de Width (largura) e Height (altura), tenham mudanças de elementos visuais. Este evento faz parte das diversas notificações que ocorrem quando uma página é colocada para fora, um processo que envolve os vários elementos da página que está sendo dimensionada e posicionada. Este processo de layout ocorre após a primeira definição de uma página (geralmente no construtor da página), uma nova passagem pode ocorre em resposta a qualquer alteração que possa afetar o layout, por exemplo, quando views são adicionados a ContentPage ou a um StackLayout, removido a partir desses objetos, ou quando as propriedades são definidas em elementos visuais que possam resultar em mudança em seus tamanhos.

Um novo layout também é acionado quando altera o tamanho da tela. Isso acontece principalmente quando o telefone é girado entre os modos retrato e paisagem.

A familiaridade completa com o sistema de layout Xamarin.Forms frequentemente acompanha o trabalho de escrever seu próprio layout &lt;View&gt; derivativos. Esta tarefa nos espera em um capítulo futuro. Até então, simplesmente o saber quando Largura e Altura mudam as propriedades é útil para trabalhar com tamanhos de objetos visuais. Você pode anexar um manipulador SizeChanged a qualquer objeto visual na página, incluindo a própria página. O programa a baixo demonstra como obter o tamanho da página e exibi-lo:

Este é o primeiro exemplo de lidar com evento neste livro, e você pode ver que os eventos são tratados na maneira normal do C # e .NET. O código, no final do construtor atribui o processador de eventos OnPageSizeChanged ao evento SizeChanged da página. O primeiro argumento para o processador de eventos (habitualmente chamado sender) é o objeto disparar o evento, neste caso, o exemplo de WhatSizePage, mas o processador de eventos que não utiliza. O manipulador de eventos nem usa o segundo argumento do evento.

Em vez disso, o manipulador de eventos acessa o elemento Label (convenientemente salva como um campo) para exibir as propriedades de Width e Height da página. O carácter Unicode na chamada String.Format é um (×) símbolo.

O evento SizeChanged não é a única oportunidade de obter o tamanho de um elemento. VisualElement também define um método virtual protegido chamado OnSizeAllocated que indica quando o elemento visual é atribuído um tamanho. Você pode substituir esse método em seu derivado ContentPage ao invés de manipular o evento SizeChanged, mas OnSizeAllocated às vezes é chamado quando o tamanho não está realmente mudando.

Aqui está o programa rodando em três plataformas:

Para o registro, estas são as fontes das telas nestas três imagens:

*   **O iPhone 6 simuladores, com dimensões de 750 × 1334 pixels.**
*   **Uma LG Nexus 5 com um tamanho de tela de 1080 × 1920 pixels.**
*   **A Nokia Lumia 925 com um tamanho de tela de 768 × 1280 pixels.**

Note-se que o tamanho vertical percebido pelo programa no Android não inclui a área ocupada pela barra de estado ou botões de baixo; o tamanho vertical no telefone Windows não inclui a área ocupada pela barra de estado.

Por padrão, todas as três plataformas respondem às mudanças de orientação do dispositivo. Se você ativar os telefones (ou emulators) 90 graus no sentido contrário, os telefones apresentar os seguintes tamanhos:

As capturas de tela para este livro são projetadas apenas para o modo retrato, assim você vai precisar transformar este aplicativo de lado para ver o que o programa se parece na paisagem. A largura 598-pixel no Android exclui a área para os botões; a altura 335-pixel exclui a barra de status, que aparece sempre acima da página. No Windows Phone, a largura 728-pixel exclui a área para a barra de status, que aparece no mesmo lugar, mas com ícones girados para refletir a nova orientação.

O programa WhatSize cria um único Label em seu construtor e define a propriedade do Text no manipulador de eventos. Essa não é a única maneira de escrever um programa desse tipo. O programa pode usar o manipulador SizeChanged para criar um Label inteiro com o novo texto e definir um novo Label com o conteúdo da página, caso o Label anterior se tornaria não referenciado e, portanto, elegível para a coleta de lixo. Mas a criação de novos elementos visuais é desnecessária e um desperdício neste programa. É melhor para o programa criar apenas um Label e apenas definir a propriedade Text para indicar novo tamanho.

Monitoramento de alterações de tamanho é a única maneira de um aplicativo Xamarin.Forms detecta mudanças de orientação, sem a obtenção de informações específicas da plataforma. A largura é maior do que a altura? Isso é paisagem. Caso contrário, é retrato.

Por padrão, os modelos do Visual Studio e Xamarin Studio para Xamarin.Forms, permitem mudanças de orientação para todas as três plataformas. Se você quiser desativar as alterações de orientação, por exemplo, se você tiver um aplicativo que simplesmente não funciona bem em modo retrato ou paisagem, você pode fazer.

Para iOS, primeiro exibir o conteúdo de Info.plist no Visual Studio ou Xamarin Studio. Na seção de Informações da implantação do iPhone, use a Supported Device Orientations para especificar quais as orientações são permitidas.

Para Android, no atributo Activity na classe MainActivity, adicione:

**ScreenOrientation = ScreenOrientation.Landscape**

**or**

**ScreenOrientation = ScreenOrientation.Portrait**

O atributo Activity gerado pela solução contém um argumento ConfigurationChanges que também se refere a tela de orientação, mas o propósito de ConfigurationChanges é inibir o reinício da atividade quando a orientação da tela ou mudanças de tamanho do telefone.

Para Windows Phone, no arquivo the MainPage.xaml.cs, altere o SupportedPageOrientation enumeration member para Portrait (retrato) ou Landscape (paisagem).