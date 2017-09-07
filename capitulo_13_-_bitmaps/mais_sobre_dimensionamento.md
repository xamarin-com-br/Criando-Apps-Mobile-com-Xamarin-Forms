## Mais Sobre Dimensionamento {#mais-sobre-dimensionamento}

Até agora, você já viu duas formas de dimensionar elementos Image:

Se o elemento Image não é limitado de qualquer forma, ele vai encher o seu recipiente, mantendo a relação de aspecto do bitmap, ou preencher a área por completo, se você definir a propriedade de aspecto Fill ou AspectFill.

Se o bitmap é menor do que o tamanho de seu recipiente e o Image é restrito horizontalmente ou verticalmente, definindo HorizontalOptions ou VerticalOptions para algo diferente de Fill, ou se o Image é colocado em um StackLayout, o bitmap é exibido em seu tamanho natural. Esse é o tamanho em pixel em iOS e dispositivos Android, mas o tamanho em unidades independentes de dispositivo em dispositivos Windows.

Você também pode controlar o tamanho, definindo WidthRequest ou HeightRequest a uma dimensão explícita em unidades independentes de dispositivo. No entanto, existem algumas limitações.

A discussão a seguir é baseada na experimentação com a amostra StackedBitmap. Que se refere a elementos Image que são verticalmente limitados por ser filho de um StackLayout vertical ou ter a propriedade VerticalOptions definida para algo diferente de Fill. Os mesmos princípios se aplicam a um elemento de Image que está restrito horizontalmente.

Se um elemento Image é verticalmente limitado, você pode usar WidthRequest para reduzir o tamanho do bitmap de seu tamanho natural, mas você não pode usá-lo para aumentar o tamanho. Por exemplo, tente definir WidthRequest a 100:

A altura resultante do bitmap é regida pela largura especificada e a relação de aspecto do bitmap, então agora a imagem é exibida com um tamanho de 100 × 75 unidades independentes de dispositivo em todas as três plataformas:

A configuração de HorizontalOptions como Center não afeta o tamanho do bitmap renderizado. Se você remover essa linha, o elemento Image será tão grande quanto a tela (como a cor de fundo do aqua irá demonstrar), mas o bitmap permanecerá o mesmo tamanho.

Não é possível utilizar WidthRequest para aumentar o tamanho do bitmap renderizado além do seu tamanho natural. Por exemplo, tente definir WidthRequest a 1000:

Mesmo com o HorizontalOptions definido para Center, o elemento Image resultante é agora maior do que o bitmap representado, como indicado pela cor de fundo:

Mas o próprio bitmap é exibido em seu tamanho natural. O StackLayout vertical é a altura preventiva do bitmap renderizado a partir do excedente da altura natural.

Para superar essa restrição do StackLayout vertical, você precisa definir o HeightRequest. Entretanto, você também vai querer deixar o HorizontalOptions em seu valor padrão de preenchimento. Caso contrário, a configuração HorizontalOptions irá impedir a largura do bitmap renderizado a partir do excedente de seu tamanho natural.

Tal como aconteceu com o WidthRequest, você pode definir o HeightRequest para reduzir o tamanho do bitmap renderizado. O código a seguir define o HeightRequest para 100 unidades independentes de dispositivo:

Observe também que a definição HorizontalOptions foi removido.

O bitmap renderizado é agora 100 unidades independentes de dispositivo de altura e com uma largura definida pela relação de aspecto. O elemento Image se estende para os lados do StackLayout:

Neste caso particular, você pode definir o HorizontalOptions para Center sem alterar o tamanho do bitmap renderizado. O elemento Image terá o tamanho do bitmap (133 x 100), e o fundo Aqua desaparecerá.

É importante deixar o HorizontalOptions em sua configuração padrão Fill quando definir o HeightRequest para um valor maior do que a altura natural do bitmap, por exemplo 250:

Agora, o bitmap renderizado é maior do que seu tamanho natural:

No entanto, esta técnica tem um perigo embutido, que é revelada quando você define o HeightRequest para 400:

Eis o que acontece: o elemento Image, de fato, tem uma altura de 400 unidades independentes de dispositivo. Mas a largura do bitmap renderizado nesse elemento Image está limitada pela largura da tela, o que significa que a altura do bitmap renderizado é menor do que a altura do elemento de imagem:

Em um programa real, você provavelmente não teria a propriedade BackgroundColor set, e em vez disso um terreno baldio de tela em branco irá ocupar a área na parte superior e inferior do bitmap prestados.

O que isto significa é que você não deve usar HeightRequest para controlar o tamanho de bitmaps em um na vertical StackLayout a menos que você escrever código que garante que HeightRequest é limitada à largura das vezes StackLayout a relação entre a altura do bitmap com a largura.

Se você sabe o tamanho do pixel do bitmap que você vai estar exibindo, uma abordagem mais fácil é definir WidthRequest e HeightRequest para esse tamanho:

Agora, o bitmap é exibido com esse tamanho em unidades independentes de dispositivo em todas as plataformas:

O problema aqui é que o bitmap não está sendo exibido na sua resolução ideal. Cada pixel do bitmap ocupa, pelo menos, dois pixels da tela, dependendo do dispositivo.

Se você quiser bitmaps de tamanho em um StackLayout vertical, de modo que eles olham aproximadamente o mesmo tamanho em uma variedade de dispositivos, use WidthRequest em vez de HeightRequest. Você viu que a busca WidthRe- em um StackLayout vertical, só pode diminuir o tamanho de bitmaps. Isso significa que você deve usar bitmaps que são maiores do que o tamanho em que eles serão processados. Isto lhe dará uma resolução mal mais opti- quando a imagem é dimensionada em unidades independentes de dispositivo. Você pode dimensionar o bitmap usando um tamanho métrica desejada em polegadas, juntamente com o número de unidades independentes de dispositivo por polegada para o dispositivo em particular, que encontramos a ser 160 para estes três dispositivos.

Aqui está um projeto muito semelhante ao StackedBitmap chamado DeviceIndBitmapSize. É o mesmo bitmap mas agora 1200 × 900 pixels, que é maior que a largura do modo retrato de mesmo de alta resolução 1920 × 1080 exibe. A largura solicitada específica da plataforma do bitmap corresponde a 1,5 polegadas:

Se a análise anterior sobre o dimensionamento é correto e tudo correr bem, este bitmap deve olhar aproximadamente o mesmo tamanho em todas as três plataformas em relação à largura da tela, bem como proporcionar maior resolução fidelidade do que o programa anterior:

Com este conhecimento sobre o dimensionamento bitmaps, agora é possível fazer um leitor pouco e-livro com as fotografias, porque o que é o uso de um livro sem imagens?

Este e-book reader exibe uma StackLayout rolagem com o texto completo do capítulo 7 das aventuras da Alice de Lewis Carroll no país das maravilhas, incluindo três de ilustrações originais de John Tenniel. O texto e as ilustrações foram baixados da Universidade do website da Adelaide. As ilustrações são incluídas como recursos incorporados no projeto MadTeaParty. Eles têm os mesmos nomes e tamanhos como aqueles no site. Os nomes referem-se ao número da página do livro original:

*   image113.jpg - 709 × 553
*   image122.jpg - 485 × 545
*   image129.jpg - 670 × 596

Lembre-se que o uso de WidthRequest para elementos de Image em um StackLayout só pode diminuir o tamanho de bitmaps prestados. Esses bitmaps não são largas o suficiente para garantir que todos eles vão encolher para um tamanho adequado em todas as três plataformas, mas vale a pena examinar os resultados de qualquer maneira porque isso é muito mais perto de um exemplo da vida real.

O programa MadTeaParty usa um estilo implícito para Imagem para definir a propriedade WidthRequest para um valor correspondente a 1,5 polegadas. Tal como no exemplo anterior, este valor é de 240.

Para os três dispositivos usados ​​para essas imagens, essa largura corresponde a:

*   480 pixels no iPhone 6
*   720 pixels no Nexus Android 5
*   540 pixels no Nokia Lumia 925 com Windows 10 Mobile

Isto significa que todas as três imagens irão diminuir de tamanho no iPhone 6, e todos eles terão uma largura processada de 240 unidades independentes de dispositivo.

No entanto, nenhuma das três imagens irá diminuir de tamanho sobre o Nexus 5 porque todos eles têm larguras de pixel mais estreitas do que o número de pixels em 1,5 polegadas. As três imagens terão uma largura processada (respectivamente) 236, 162, e 223 unidades independentes de dispositivo no Nexus 5\. (Que é a largura de pixel di- fornecida pelos 3.)

No dispositivo Windows Mobile 10, dois vai encolher e um não.

Vamos ver se as previsões estiverem corretas. O arquivo XAML inclui uma configuração BackgroundColor no elemento raiz que as cores a página inteira brancos, como é apropriado para um livro. As definições de estilo são confinadas a um dicionário Resources na StackLayout. Um estilo para o título do livro baseia-se na vice-TitleStyle de- mas com texto preto e centrado, e dois estilos implícitos para Etiqueta e Imagem servir ao estilo a maioria dos elementos do rótulo e todos os três elementos de imagem. Apenas os primeiros e últimos parágrafos do texto do capítulo são mostrados nesta lista do arquivo XAML:

Os três elementos de imagem simplesmente fazer referência os três recursos incorporados e recebem uma configuração da propriedade WidthRequest através do estilo implícito:

&lt;Image Source=&quot;{local:ImageResource MadTeaParty.Images.image113.jpg}&quot; /&gt;

&lt;Image Source=&quot;{local:ImageResource MadTeaParty.Images.image122.jpg}&quot; /&gt;

&lt;Image Source=&quot;{local:ImageResource MadTeaParty.Images.image129.jpg}&quot; /&gt;

Aqui está a primeira tela:

É bastante consistente entre as três plataformas, mesmo que seja exibido em sua largura natural de 709 pixels no Nexus 5, mas isso é muito perto dos 720 pixels que uma largura de 240 unidades independentes de dispositivo implica.

A diferença é muito maior com a segunda imagem:

Isso é exibido em seu tamanho pixel no Nexus 5, o que corresponde a 162 unidades independentes de dispositivo, mas é exibido com uma largura de 240 unidades no iPhone 6 eo Nokia Lumia 925.

Embora as imagens não parecem ruim em qualquer uma das plataformas, levando-os todos sobre o mesmo tamanho exigiria começando com bitmaps maiores.