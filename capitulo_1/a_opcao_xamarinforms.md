## A opção Xamarin.Forms {#a-op-o-xamarin-forms}

Xamarin.Forms suporta cinco plataformas de aplicativos distintas:

* iOS para programas que são executados no iPhone, iPad e iPod Touch.
* Android para programas que funcionam em telefones e tablets Android.
* A Universal Windows Platform \(UWP\) para aplicativos que são executados no Windows 10 ou
   Windows 10 Mobile.
* O Windows Runtime API do Windows 8.1.
* A API do Windows Runtime do Windows Phone 8.1.

Neste livro, "Windows" ou "Windows Phone" geralmente serão usados como um termo genérico para descrever todas três das plataformas da Microsoft.

No caso geral, uma aplicação Xamarin.Forms é composta de três projetos separados para as três plataformas móveis, com um quarto projeto que contém o código comum muito parecido com o diagrama que apareceu na seção anterior. No entanto, os três projetos de plataformas em um aplicativo Xamarin.Forms são tipicamente muito pequenos, muitas vezes consistindo de apenas stubs com um pouco de código de inicialização. O Projeto de código de código compartilhado, ou no projeto Class Library, contém a maior parte do aplicativo, incluindo o código da interface pelo usuário:

![An illustration showing the interrelationships between the Visual Studio or Xamarin Studio Xamarin.Forms projects, the Xamarin libraries, and the platform APIs.](../assets/an_illustration_showing_the_interre.png)

As bibliotecas** Xamarin.Forms.Core **e **Xamarin.Forms.Xaml i**mplementam a API Xamarin.Forms. Dependendo da plataforma, Xamarin.Forms.Core seguida, faz uso de uma das bibliotecas Xamarin.Forms.Platform. Essas bibliotecas são na maior parte uma coleção de classes chamados representantes que transformam os objetos de interface do usuário Xamarin.Forms na interface do usuário específico da plataforma.

O restante do diagrama é o mesmo que o mostrado anteriormente.

Por exemplo, suponha que você precisa do objeto de interface do usuário discutido anteriormente que permite ao usuário alternar um valor booleano. Ao programar Xamarin.Forms, isso é chamado de Switch, e uma classe chamada Change é implementada na biblioteca Xamarin.Forms.Core. Na interfaces  individuais para as três plataformas, este switch é mapeado para um UISwitch no iPhone, um interruptor no Android, e um telefone ToggleSwitchButton no Windows.

**Xamarin.Forms.Core** também contém uma classe chamada Slider para a exibição de uma barra horizontal que o usuário manipula para escolher um valor numérico. Na interfaces  nas bibliotecas específicas da plataforma, é mapeado para um UISlider no iPhone, a SeekBar no Android, e um telefone deslizante no Windows.

Isto significa que quando você escreve um programa Xamarin.Forms que tem um interruptor ou um Slider, o que está realmente exibido é o objeto correspondente implementado em cada plataforma.

Aqui está um pequeno programa Xamarin.Forms contendo uma etiqueta dizendo "Olá, Xamarin.Forms!", Um botão que diz "Clique-me!", Um interruptor e um Slider. O programa está sendo executado em \(a partir da esquerda para a direita\) o iPhone, Android e Windows Phone:

![](/assets/1.1-hello.PNG)

A captura de tela do iPhone é de um simulador de iPhone 6 executando o iOS 9.2. O telefone Android é um LG Nexus 5 executando a versão de Android 6. O dispositivo móvel do Windows 10 é um Nokia Lumia 935 com uma visualização técnica do Windows 10.

Você encontrará três screenshots como este ao longo deste livro. Eles estão sempre na mesma ordem - iPhone, Android e Windows 10 Mobile - e eles estão sempre executando o mesmo programa.

Como você pode ver, o botão, alternar e controle deslizante todos têm aparências diferentes nos três telefones, porque todos eles são renderizados com o objeto específico para cada plataforma.

O que é ainda mais interessante é a inclusão neste programa de seis objetos ToolBarItem, três identificados como itens principais com ícones e três como itens secundários sem ícones. No iPhone, estes são processados ​​com objetos UIBarButtonItem como os três ícones e três botões na parte superior da página. No Android, os três primeiros são renderizados como itens em um ActionBar, também no topo da página. No Windows 10 Mobile, eles são percebidos como itens no CommandBar na parte inferior da página.

O Android ActionBar tem uma reticência vertical e o CommandBar Universal da Plataforma Windows tem uma reticência horizontal. Tocar nesta reticência faz com que os itens secundários sejam exibidos da maneira apropriada para essas duas plataformas:

![](/assets/1.2-hello.PNG)

