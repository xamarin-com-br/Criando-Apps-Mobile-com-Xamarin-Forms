## Ícones para Android. {#cones-para-android}

O site Android tem uma coleção download de ícones da barra de ferramentas disponíveis neste URL:

http://developer.android.com/design/downloads

Baixe o arquivo ZIP identificado como Barra de ação Icon Pack.

O conteúdo descompactados estão organizados em dois diretórios principais: Core_Icons (23 imagens) e ação ícones da barra (144 imagens). Estes são todos os arquivos PNG, ea barra de ícones de ação vêm em quatro tamanhos diferentes, indicados pelo nome do diretório:

*   drawable-mdpi (médio DPI) - 32 pixels quadrados
*   drawable-hdpi (alta DPI) - 48 pixels quadrados
*   drawable-xhdpi (extra alta DPI) - 64 pixels quadrados
*   drawable-xxhdpi (DPI alta extra extra) - 96 pixels quadrados

Estes nomes de diretório são as mesmas que as pastas de recursos em seu projeto Android e implica que os ícones da barra de ferramentas render em 32 unidades independentes de dispositivo, ou cerca de um quinto de polegada.

A pasta core_Icons também organiza seus ícones em quatro diretórios com os mesmos quatro tamanhos, mas estes diretórios são nomeados mdpi, hdpi, xdpi, e sem escala.

  A pasta de Ação Bar Icons tem uma organização diretório adicional usando os nomes holo_dark

e holo_light:

*   imagem de primeiro plano holo_dark-branco sobre um fundo transparente
*   imagem de primeiro plano holo_light-preto em um fundo transparente

A palavra &quot;holo&quot; significa &quot;holográfico&quot;, e refere-se ao nome usa o Android para os seus temas de cores. Alt Hough os ícones holo_light são muito mais fáceis de ver no Finder e Windows Explorer, para a maioria dos fins (e especialmente para itens da barra de ferramentas), você deve usar os ícones holo_dark. (Claro, se você sabe como alterar o seu tema aplicativo no arquivo AndroidManifest.xml, então elas provavelmente também sabem usar a outra cobrança de ícone.)

A pasta core_Icons contém apenas ícones com foregrounds brancas sobre um fundo transparente.

Para o programa ToolbarDemo, três ícones foram escolhidos a partir do diretório holo_dark em todas as quatro resoluções. Estes foram copiados para as subpastas apropriadas do diretório de recursos no projeto Android:

*   A partir do diretório 01_core_edit, os arquivos chamado ic_action_edit.png
*   A partir do diretório 01_core_search, os arquivos chamado ic_action_search.png
*   A partir do diretório 01_core_refresh, os arquivos chamado ic_action_refresh.png

Verifique as propriedades desses arquivos PNG. Eles devem ter um Build Action do AndroidResource. Ícones para as plataformas Windows Runtime

Se você tiver uma versão do Visual Studio instalado para Windows Phone 8, você pode encontrar uma coleção de arquivos PNG adequado para ToolbarItem no seguinte diretório em seu disco rígido:

C: \ Arquivos de Programas (x86) \ Microsoft SDKs \ Windows Phone \ v8.0 \ ícones que você pode usar estes para todas as plataformas Windows Runtime.

Há dois subdiretórios, claro e escuro, cada um contendo os mesmos 37 imagens. Tal como acontece com An- droid, os ícones no diretório escuro têm foregrounds brancas em fundos transparentes, e os ícones no diretório Luz tem foregrounds pretas em fundos transparentes. Você deve usar os do diretório escuro para Windows Phone 8.1 e o diretório Luz para Windows 10 Mobile.

As imagens são um quadrado uniformes 76 pixels, mas foram projetados para aparecer dentro de um círculo. De fato, um dos arquivos é nomeado basecircle.png, que pode servir como um guia se você gostaria de projetar seu próprio, portanto, não são realmente apenas 36 ícones utilizáveis ​​na recolha e alguns deles são os mesmos.

Geralmente, em um projeto Windows Runtime, arquivos como estes são armazenados na pasta Assets (que já existe no projeto) ou uma pasta chamada Images. Os seguintes bitmaps foram adicionados a uma pasta idades Im- em todas as três plataformas Windows:

*   edit.png
*   feature.search.png
*   refresh.png

Para a plataforma Windows 8.1 (mas não o Windows Phone 8.1 plataforma), os ícones são necessários para todos os itens da barra de ferramentas, então as seguintes bitmaps foram adicionados à pasta Imagens desse projeto:

*   Icon1F435.png
*   Icon1F440.png
*   Icon1F52D.png

Estes foram gerados em um programa do Windows a partir da fonte Segoe UI Symbol, que suporta caracteres emoji. O número hexadecimal de cinco dígitos no nome do arquivo é o ID Unicode para os caracteres.

Quando você adicionar ícones para um projeto Windows Runtime, verifique se o Build Action é o conteúdo. Ícones para dispositivos iOS

Esta é a plataforma mais problemático para ToolbarItem. Se você está programando diretamente para a API nativa iOS, um grupo de constantes permitem que você selecione uma imagem para UIBarButtonItem, que é a implementação iOS subjacente de ToolbarItem. Mas para o Xamarin.Forms ToolbarItem, você vai precisar para obter ícones de outra fonte, talvez licenciamento de uma coleção como a que está em glyphish.com, ou fazer o seu próprio.

Para melhores resultados, você deve fornecer dois ou três arquivos de imagem para cada item da barra de ferramentas na pasta Recursos. Uma imagem com um nome de arquivo, como image.png deve ser de 20 pixels quadrados, enquanto a mesma imagem também deve ser fornecido em uma dimensão de 40-pixel quadrado com o nome image@2x.png e como um bitmap 60- pixel quadrado chamado image@3x.png.

Aqui está uma coleção de ícones de utilização livre livres utilizados para o programa no Capítulo 1 e para o programa ToolbarDemo neste capítulo:

http://www.smashingmagazine.com/2010/07/14/gcons-free-all-purpose-icons-for-designers-and-de- velopers-100-icons-psd/  

No entanto, eles são uniformemente 32 pixels quadrados, e alguns básicos estão em falta. Independentemente disso, os seguintes aos três bitmaps foram copiados para a pasta Resources no projeto iOS sob a suposição de que eles serão devidamente dimensionados:

*   edit.png
*   search.png
*   reload.png

Outra opção é usar ícones Android a partir do diretório holo_light e dimensionar a maior imagem para os vários tamanhos iOS.

Para ícones da barra de ferramentas em um projeto do iOS, o Criar ação deve ser BundleResource.

Aqui está o arquivo ToolbarDemo XAML mostrando os vários objetos ToolbarItem adicionados à coleção ToolbarItems da página. O x: TypeArguments atributo para OnPlatform deve ser FileImageSource neste caso, porque esse é o tipo da propriedade Ícone de ToolbarItem. Os três itens marcados como secundário têm apenas o conjunto de propriedades de texto e não a propriedade ícone.

O elemento raiz tem uma propriedade título definido na página. Isso é exibido nas telas iOS e Android quando a página é instanciada como um NavigationPage (ou navegou a partir de uma página navegação-):

Embora o elemento OnPlatform implica que existem os ícones secundárias para todas as plataformas Windows Runtime, eles não fazem, mas nada de ruim acontece se o arquivo de ícone particular, está faltando no projeto.

Todos os eventos Clicked têm o mesmo processador atribuído. Você pode usar manipuladores exclusivos para os itens, é claro. Este manipulador apenas exibe o texto da ToolbarItem usando a etiqueta centrado:

As imagens mostram os itens ícone da barra (e para iOS, os itens de texto) e o rótulo centrado com o item mais recentemente clicado:

Se tocar nas reticências na parte superior da tela Android ou nas reticências no canto inferior direito da tela móvel do Windows 10, os itens de texto são exibidos e, além disso, os itens de texto associados com os ícones também são exibidos no Windows 10 mobile:

Independentemente da plataforma, a barra de ferramentas é a maneira padrão para adicionar comandos comuns a um aplicativo de telefone.