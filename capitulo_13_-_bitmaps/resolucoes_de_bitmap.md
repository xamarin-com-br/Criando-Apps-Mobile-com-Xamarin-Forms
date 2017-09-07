## Resoluções de bitmap {#resolu-es-de-bitmap}

O nome do arquivo bitmap iOS especificado no PlatformBitmaps é Icon-Small-40.png, mas se você olhar na pasta recursos do projeto iOS, você verá três arquivos com variações desse nome. Todos eles têm tamanhos diferentes:

*   Icon-Small-40.png - 40 pixels quadrados
*   Icon-Small-40@2x.png - 80 pixels quadrados
*   Icon-Small-40@3x.png - 120 pixels quadrado

Como você descobriu no início deste capítulo, quando uma imagem é uma criança de um StackLayout, iOS exibe o bitmap em seu tamanho do pixel com um mapeamento um-para-um entre os pixels do bitmap e os pixels da tela. Esta é a exibição óptima de um mapa de bits.

No entanto, no simulador iPhone 6 utilizadas na captura de tela, a imagem tem um tamanho processado de 40 unidades independentes de sitivo. No iPhone 6 há dois pixels por unidade independente do dispositivo, o que significa que o bitmap real que está sendo exibido na tela que não é Icon-Small-40.png, mas Icon-Small-40@2x.png, que é duas vezes 40, ou 80 pixels quadrado.

Se você em vez disso executar o programa no iPhone 6 Plus-que tem uma unidade independente de dispositivo igual a três pixels, pois você novamente ver um tamanho processado de 40 pixels, o que significa que o bitmap Icon-Small-40@3x.png é exibido. Agora tente isso no simulador iPad 2\. O iPad 2 tem um tamanho de tela de apenas 768 × 1024, e unidades independentes de dispositivo são os mesmos pixels. Agora, o bitmap Icon-Small-40.png é exibido, eo tamanho processado ainda é 40 pixels.

Isso é o que você quer. Você quer ser capaz de controlar o tamanho processado de bitmaps em unidades pendentes de dispositivo de dente, porque é assim que você pode atingir tamanhos de bitmap sensivelmente semelhantes em diferentes dispositivos e plataformas. Quando você especifica o bitmap 40.png Icon-Small-, você quer que bitmap para ser processado como 40 independentes de dispositivo unidades, ou cerca de um quarto de polegada-em todos os dispositivos iOS. Mas se o programa está sendo executado em um dispositivo Apple Retina, você não quer um bitmap 40-pixel quadrado esticado para ser de 40 unidades independentes de sitivo. Por fidelidade visual máximo, você quer um bitmap maior resolução apresentada, com um mapeamento um-para-um dos pixels de bitmap para a tela pixels.

Se você olhar no diretório Resources Android, você vai encontrar quatro versões diferentes de um bitmap chamado icon.png. Estes são armazenados em diferentes subpastas de recursos:

*   drawable / icon.png - 72 pixels quadrados
*   drawable-hdpi / icon.png - 72 pixels quadrados
*   drawable-xdpi / icon.png - 96 pixels quadrados
*   drawable-xxdpi / icon.png - 144 pixels quadrado

Independentemente do dispositivo Android, o ícone é processado com um tamanho de 48 unidades independentes de dispositivo. No Nexus 5 utilizadas na captura de tela, há três pixels para a unidade independente do dispositivo, o que significa que o bitmap realmente exibido nessa tela é a única na pasta drawable-xxdpi, que é de 144 pixels quadrado.

O que é agradável sobre ambos iOS e Android é que você só precisa fornecer bitmaps de vários tamanhos-e dar-lhes os nomes corretos ou armazená-los nas pastas corretas e o sistema operacional escolhe a melhor imagem para a resolução específica do dispositivo.

A plataforma Windows Runtime tem uma facilidade similar. No projeto UWP você verá nomes de arquivos que incluem escala-200; por exemplo, Square150x150Logo.scale-200.png. O número após a escala palavra é uma percentagem, e embora o nome do arquivo parece indicar que este é um mapa de bits de 150 x 150, a imagem é na verdade, duas vezes maior: 300 × 300\. No projeto do Windows você verá nomes de arquivos que incluem escala-100 e no projeto WinPhone você verá scale-240.

No entanto, você viu que Xamarin.Forms no Windows Runtime exibe bitmaps em seus tamanhos de- independente de vício, e você ainda vai precisar para tratar as plataformas Windows um pouco diferente. Mas em todas as três plataformas que você pode controlar o tamanho de bitmaps em unidades independentes de dispositivo.

Ao criar suas próprias imagens específicas da plataforma, siga as orientações nas próximas três seções.