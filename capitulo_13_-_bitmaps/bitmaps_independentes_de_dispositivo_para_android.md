## Bitmaps independentes de dispositivo para Android {#bitmaps-independentes-de-dispositivo-para-android}

Para Android, bitmaps são armazenados em vários subpastas de recursos que correspondem a um pixel de resolução de tela. Android define seis nomes de diretórios diferentes para seis níveis diferentes de dispositivo de resolução:

*   drawable-LDPI (baixo DPI) para 120 dispositivos DPI (0,75 pixels ao DIU)
*   drawable-mdpi (médio) para dispositivos de 160 dpi (1 pixel ao DIU)
*   drawable-hdpi (alto) para 240 dispositivos DPI (1,5 pixels ao DIU))
*   drawable-xhdpi (extra alta) para 320 dispositivos DPI (2 pixels para a DIU)
*   drawable-xxhdpi (extra extra alta) para 480 dispositivos DPI (3 pixels para a DIU)
*   drawable-xxxhdpi (três elevações adicionais) para dispositivos de 640 dpi (4 pixels para o DIU)

Se você quer um bitmap chamado myimage.jpg para renderizar como um quadrado de uma polegada na tela, você pode fornecer até seis versões deste bitmap usando o mesmo nome em todos esses diretórios. O tamanho deste mapa de bits de uma polegada quadrada em pixels é igual a DPI associada a esse diretório:

*   drawable-LDPI / myimage.jpg - 120 pixels quadrado
*   drawable-mdpi / myimage.jpg - 160 pixels quadrado
*   drawable-hdpi / myimage.jpg - 240 pixels quadrado
*   drawable-xhdpi / myimage.jpg - 320 pixels quadrado
*   drawable-xxdpi / myimage.jpg - 480 pixels quadrado
*   drawable-xxxhdpi / myimage.jpg - 640 pixels quadrado

O bitmap serão renderizados como 160 unidades independentes de dispositivo.

Você não é obrigado a criar mapas de bits para as seis resoluções. O projeto Android criado pelo molde Xamarin.Forms inclui apenas amovível-HDPI, amovível-xhdpi, e amovível-xxdpi, bem como uma pasta amovível desnecessária sem sufixo. Estas abrangem os dispositivos mais comuns. Se o sistema operacional Android não encontrar um bitmap da resolução desejada, ele vai cair de volta para um tamanho que está disponível e escalá-lo.