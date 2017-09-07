## Bitmaps independentes de dispositivo para iOS {#bitmaps-independentes-de-dispositivo-para-ios}

O esquema de nomenclatura iOS para bitmaps envolve um sufixo no nome de arquivo. O sistema operacional busca um bitmap em particular com o nome do arquivo subjacente baseada na resolução de pixel aproximada do dispositivo:

*   Sem sufixo para dispositivos 160 dpi (1 pixel para a unidade independente de dispositivo)
*   @ 2x sufixo para 320 dispositivos DPI (2 pixels para a DIU)
*   @ 3x sufixo: 480 dispositivos DPI (3 pixels para a DIU)

Por exemplo, suponha que você queira um bitmap chamado myimage.jpg a mostrar-se como cerca de uma polegada quadrada na tela. Você deve fornecer três versões deste bitmap:

*   myimage.jpg - 160 pixels quadrado
*   MyImage@2x.jpg~~number=plural - 320 pixels quadrado
*   MyImage@3x.jpg~~number=plural - 480 pixels quadrado

O bitmap serão renderizados como 160 unidades independentes de dispositivo. Para tamanhos prestados menor que uma polegada, de- vinco os pixels proporcionalmente.

Ao criar esses bitmaps, comece com o maior. Então você pode usar qualquer utilitário de edição de bitmap para reduzir o tamanho do pixel. Para algumas imagens, você pode querer ajustar ou completamente redesenhar as versões menores.

Como você deve ter notado ao examinar os vários arquivos de ícone que o modelo Xamarin.Forms inclui com o projeto iOS, nem todo bitmap vem em todas as três resoluções. Se iOS não consegue encontrar um bitmap com o sufixo particular, ele quer, ele vai cair para trás e usar um dos outros, escalando o bitmap cima ou para baixo no processo.