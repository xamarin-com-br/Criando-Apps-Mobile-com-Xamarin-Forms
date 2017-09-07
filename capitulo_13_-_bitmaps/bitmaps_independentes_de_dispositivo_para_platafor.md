## Bitmaps independentes de dispositivo para plataformas Windows Runtime {#bitmaps-independentes-de-dispositivo-para-plataformas-windows-runtime}

O Windows Runtime suporta um esquema de bitmap de nomeação que permite incorporar um fator de escala de pixels por unidade independente de dispositivo expresso como uma percentagem. Por exemplo, por um mapa de bits de uma polegada quadrada apontado a um dispositivo que tem dois pixels para o aparelho, utilizar o nome:

*   MyImage.scale-200.jpg - 320 pixels quadrado

A documentação do Windows não é clara sobre as percentagens reais que você pode usar. Quando a construção de um programa, às vezes você verá mensagens de erro na janela de saída em relação percentagens que não são suportados na plataforma particular.

No entanto, dado que Xamarin.Forms exibe bitmaps Windows Runtime em seus tamanhos independentes de dispositivo, esta facilidade é de uso limitado nesses dispositivos.

Vamos olhar para um programa que realmente faz fornecer bitmaps personalizados de vários tamanhos para as três plataformas. Esses bitmaps são destinados a serem prestados cerca de uma polegada quadrada, o que é aproximadamente metade da largura da tela do telefone no modo retrato.

Este programa ImageTap cria um par de objetos semelhantes a botões rudimentares, tappable que não exibir o texto, mas um bitmap. Os dois botões que ImageTap cria pode substituir a tradicionais botões OK e Cancelar, mas talvez você queira usar rostos de pinturas famosas para os botões. Talvez você deseja que o botão OK para exibir a face de Vênus de Botticelli e o botão para exibir o homem tressed dis- no de Edvard Munch O Grito Cancelar.

No código de exemplo para este capítulo é um diretório chamado Imagens que contém essas imagens, nomeado Venus_xxx.jpg e Scream_xxx.jpg, onde o xxx indica o tamanho do pixel. Cada imagem é em oito tamanhos diferentes: 60, 80, 120, 160, 240, 320, 480, e 640 pixels quadrado. Além disso, alguns dos arquivos têm nomes de Venus_xxx_id.jpg e Scream_xxx_id.jpg. Essas versões têm o tamanho de pixel real exibido no canto inferior direito da imagem, de modo que podemos ver na tela exatamente o bitmap o sistema operativo tenha selecionado.

Para evitar confusão, os bitmaps com os nomes originais foram adicionados às pastas do projeto ImageTap primeiro lugar, e em seguida, eles foram renomeados dentro do Visual Studio.

Na pasta Recursos do projeto iOS, os seguintes arquivos foram renomeados:

*   Venus_160_id.jpg tornou Venus.jpg
*   Venus_320_id.jpg porque Venus@2x.jpg
*   Venus_480_id.jpg tornou Venus@3x.jpg

Isso foi feito da mesma forma para os bitmaps Scream.jpg.

Nas várias subpastas da pasta Recursos do projeto Android, os seguintes arquivos foram renomeados:

*   Venus_160_id.jpg tornou drawable-mdpi / Venus.jpg
*   Venus_240_id.jpg tornou drawable-hdpi / Venus.jpg
*   Venus_320_id.jpg tornou drawable-xhdpi / Venus.jpg
*   Venus_480_id.jpg tornou drawable_xxhdpi / Venus.jpg

E da mesma forma para os bitmaps Scream.jpg.

Para a 8.1 projecto Windows Phone, os arquivos Venus_160_id.jpg e Scream_160_id.jpg foram copiados a uma pasta Imagens e renomeado Venus.jpg e Scream.jpg.

O projeto do Windows 8.1 cria um executável que é executado não em telefones, mas em tábuas e desktops. Estes dispositivos têm tradicionalmente assumiu uma resolução de 96 unidades por polegada, de modo que o _100_id.jpg Vênus e arquivos Scream_100_id.jpg foram copiados para uma pasta de Imagens e renomeado Venus.jpg e Scream.jpg.

O projeto UWP tem como alvo todos os fatores de forma, por isso vários bitmaps foram copiados para uma pasta de Imagens e renomeado para que os bitmaps quadrados de 160 pixels seriam usados em telefones, e os bitmaps quadrados de 100 pixels seria usado em tablets e telas de desktop:

*   Venus_160_id.jpg tornou Venus.scale-200.jpg
*   Venus_100_id.jpg tornou Venus.scale-100.jpg E da mesma forma para os bitmaps Scream.jpg.

Cada um dos projetos requer um Build Action diferente para esses bitmaps. Isso deve ser definido automaticamente quando você adiciona os arquivos para os projetos, mas você definitivamente querer verificar novamente para garantir que o Build Action está definido corretamente:

*   iOS: BundleResource
*   Android: AndroidResource
*   Windows Runtime: Conteúdo

Você não tem que memorizar estes. Em caso de dúvida, basta verificar a criar ação para os bitmaps incluí- pelo modelo de solução Xamarin.Forms nos projetos de plataforma.

O arquivo XAML para o programa ImageTap coloca cada um dos dois elementos de imagem em um ContentView que é de cor branca de um estilo implícito. Este ContentView branco é inteiramente coberto pela imagem, mas (como você verá) que entra em jogo quando o programa pisca a imagem para sinalizar que tem sido aproveitado.

O arquivo XAML usa OnPlatform para selecionar os nomes dos ficheiros de recursos da plataforma. Observe que o atributo x: TypeArguments de OnPlatform está definido para ImageSource porque este tipo deve corresponder exatamente o tipo da propriedade de destino, que é a propriedade Fonte de imagem. ImageSource define uma conversão implícita de corda para si, por isso, especificando os nomes dos arquivos é suficiente. (A lógica para essa conversão implícita primeiro verifica se a string tem um prefixo URI. Se não, ele assume que a string é o nome de um arquivo incorporado no projeto de plataforma.)

Se você quiser evitar o uso de OnPlatform inteiramente em programas que usam bitmaps plataforma, você pode colocar os mapas de bits do Windows no diretório raiz do projeto, em vez de em uma pasta.

Se tocar num destes botões faz duas coisas: O manipulador de Tapped define a propriedade opacidade da imagem para 0,75, o que resulta em revelar parcialmente o fundo ContentView branco e simulem um flash. Um temporizador restaura a opacidade para o valor padrão de um décimo de segundo mais tarde. O manipulador de Tapped também exibe o tamanho processado do elemento Image:

Isso tamanho processado comparados com os tamanhos de pixel sobre os bitmaps confirma que as três plataformas de fato selecionaram o bitmap ideal:

Estes botões ocupar aproximadamente metade da largura da tela em todas as três plataformas. Este dimensionamento é inteiramente baseado no tamanho dos próprios mapas de bits, sem qualquer informação adicional de colagem ou no código de marcação.