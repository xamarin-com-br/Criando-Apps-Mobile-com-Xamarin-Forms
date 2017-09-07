## Recursos Incorporados {#recursos-incorporados}

Acessar bitmaps através da Internet é conveniente, mas às vezes não é ideal. O processo requer uma ligação à Internet, uma garantia de que os bitmaps não foram movidos, e algum tempo para download. Para o acesso rápido e garantido aos bitmaps, podemos deixá-los ligados diretamente na aplicação.

Se você precisa de acesso a imagens que não são de uma plataforma específica, você pode incluir os bitmaps como recursos incorporados em um projeto shared Portable Class Library e acessá-los com o método ImageSource.From-Resource. A solução ResourceBitmapCode demonstra como fazê-lo.

O projeto ResourceBitmapCode PCL dentro desta solução tem uma pasta chamada Images que contém dois bitmaps, nomeado ModernUserInterface.jpg (um grande bitmap) e ModernUserInter-face256.jpg (a mesma imagem, mas com uma largura de 256 pixels).

Ao adicionar qualquer tipo de recurso incorporado a um projeto PCL, certifique-se de definir o Build Action do recurso para EmbeddedResource. Isto é crucial.

No código, você define a propriedade Source de um elemento Image para um objeto ImageSource retornado do método estático ImageSource.FromResource. Este método requer o ID do recurso. O ID do recurso consiste na combinação do nome do conjunto seguido em uma parte, em seguida, o nome da pasta seguido de outra parte, e em seguida o nome do arquivo, que contém outra parte com a extensão do arquivo. Para este exemplo, o ID do recurso para acessar a menor das duas imagens no programa ResourceBitmapCode é:

**ResourceBitmapCode.Images.ModernUserInterface256.jpg**

O código neste programa referência o menor bitmap e também define a HorizontalOptions e a VerticalOptions do elemento Image para Center:

Como você pode ver, o bitmap, neste instancia não é esticado para preencher a página:

Um bitmap não é esticado para preencher o seu recipiente se:

*   É menor do que o recipiente, e
*   Os propriedades _VerticalOptions_ e _HorizontalOptions_ do elemento _Image_ não estão definidos para Fill, ou se o _Image_ é filho de um _StackLayout_.

Se você comentar as configurações VerticalOptions e HorizontalOptions, ou se você faz referência ao bitmap grande (que não tem o &quot;256&quot; no final do seu nome de arquivo), a imagem voltará a esticar para encher o recipiente.

Quando um bitmap não é esticado para se ajustar o seu recipiente, ele deve ser exibido em um determinado tamanho. Qual é esse tamanho?

No iOS e Android, o bitmap é exibido no seu tamanho em pixel. Em outras palavras, o bitmap é renderizado com um mapeamento um-para-um entre os pixels do bitmap e os pixels do monitor de vídeo. O simulador do iPhone 6 usado para essas imagens tem uma largura de tela de 750 pixels, e você pode ver que a largura de 256 pixels do bitmap é cerca de um terço da largura. O telefone Android aqui é um Nexus 5, que tem uma largura de pixel de 1080, e o bitmap é cerca de um quarto de sua largura.

Nas plataformas Windows Runtime, no entanto, o bitmap é exibido em unidades independentes de dispositivo - neste exemplo, 256 unidades independentes de dispositivo. O Nokia Lumia 925 utilizado para estas telas tem uma largura de 768, que é aproximadamente o mesmo que o iPhone 6\. No entanto, a largura da tela neste telefone Windows 10 Mobile é 341 unidades independentes de dispositivo, e você pode ver que o bitmap renderizado é muito mais amplo do que nas outras plataformas.

Esta discussão sobre dimensionamento de bitmaps continua na próxima seção.

Como você faz referência a um bitmap armazenado como um recurso incorporado a partir do XAML? Infelizmente, não existe uma classe ResourceImageSource. Se houvesse, você provavelmente tentaria instanciar essa classe XAML entre as tags Image.Source. Mas isso não é uma opção.

Você pode considerar o uso x:FactoryMethod para chamar ImageSource.FromResource, mas isso não vai funcionar. Na implementação atual, o método ImageSource.FromResource requer que o recurso bitmap esteja no mesmo conjunto que o código que chama o método. Quando você usa x:FactoryMethod para chamar ImageSource.FromResource, a chamada é feita a partir da montagem Xamarin.Forms.Xaml.

O que vai funcionar é uma extensão de marcação XAML muito simples. Aqui está um projeto denominado Stacked-Bitmap:

ImageResourceExtension tem uma única propriedade denominada Source que você define o ID do recurso. O método ProvideValue simplesmente chama ImageSource.FromResource com a propriedade Source. Como é comum para marcação de extensões de propriedade única, Source é também a propriedade de conteúdo da classe. Isso significa que você não precisa incluir explicitamente &quot;Source =&quot; quando você está usando a sintaxe curly-braces para extensões de marcação XAML.

Mas cuidado: você não pode mover esta classe ImageResourceExtension a uma biblioteca, como Xamarin.FormsBook.Toolkit. A classe deve fazer parte do mesmo conjunto que contém o recurso incorporado que você deseja carregar, que é geralmente uma aplicação Portable Class Library.

Aqui está o arquivo XAML do projeto StackedBitmap. Um elemento Image compartilha uma StackLayout com dois elementos do Label:

O prefixo local refere-se ao namespace StackedBitmap na montagem StackedBitmap. A propriedade Source do elemento Image está definida para a extensão de marcação ImageResource, que se referência um bitmap armazenado na pasta Images do projeto PCL e sinalizado como um EmbeddedResource. O bitmap é de 320 pixels de largura e 240 pixels de altura. O Image também tem a propriedade BackgroundColor; isso vai permitir-nos ver o tamanho inteiro da Image dentro da StackLayout.

O elemento Image tem o seu evento SizeChanged definido como um manipulador no arquivo code-behind:

O tamanho do elemento Image é limitado verticalmente pela StackLayout, portanto, o bitmap é exibido em seu tamanho em pixel (no iOS e Android) e em unidades independentes de dispositivo no Windows Phone. A Label exibe o tamanho do elemento Image em unidades independentes de dispositivo, que diferem em cada plataforma:

A largura do elemento Image exibido pelo Label alinhado abaixo inclui o fundo aqua igualmente à largura da página em unidades independentes de dispositivo. Você pode usar as configurações Aspect de Fill ou AspectFill para fazer o bitmap preencher inteiramente essa área aqua.

Se você preferir que o tamanho do elemento Image seja do mesmo tamanho que o bitmap processado em unidades independentes de dispositivo, você pode definir a propriedade HorizontalOptions do Image para algo diferente do que o valor padrão de Fill:

Agora, o Label alinhado abaixo exibe apenas a largura do bitmap prestado. Configurações da propriedade Aspect não terão efeito:

Vamos referir-se a este Image renderizado como a sua dimensão natural, porque é baseado no tamanho do bitmap a sendo exibido.

O iPhone 6 tem uma largura de pixels de 750 pixels, mas como você descobriu ao executar o programa WhatSize no Capítulo 5, os aplicativos percebem uma largura de tela de 375\. Temos dois pixels por unidade independente do dispositivo, então um bitmap com uma largura de 320 pixels é exibido com uma largura de 160 unidades.

O Nexus 5 tem uma largura de pixel de 1080, mas os aplicativos percebem uma largura de 360, por isso temos três pixels por unidade independente de dispositivo, como a largura da imagem de 107 unidades confirma.

Em ambos os dispositivos iOS e Android, quando um bitmap é exibido em seu tamanho natural, há um mapeamento um-para-um entre os pixels do bitmap e os pixels do display. Nos dispositivos Windows Runtime, porém, que não é o caso. O Nokia Lumia 925 utilizado para estas telas tem uma largura de pixel de 768\. Ao executar o sistema operacional Windows 10 Mobile, existem 2,25 pixels por unidade de dispositivo independente, para que os aplicativos percebam uma largura de tela de 341\. Mas as 320 × 240 pixels bitmap são exibidos em um tamanho de 320 × 240 unidades independentes de dispositivo.

Esta inconsistência entre o Windows Runtime e as outras duas plataformas é realmente benéfica quando você está acessando bitmaps a partir dos projetos de plataformas individuais. Como você verá, iOS e Android incluem uma funcionalidade que lhe permite fornecer diferentes tamanhos de bitmaps para diferentes resoluções de dispositivo. Em efeito, isso permite que você especifique tamanhos de bitmap em unidades independentes de dispositivo, o que significa que os dispositivos do Windows são consistentes com esses esquemas.

Mas ao usar bitmaps independentes de plataforma, você provavelmente vai querer o tamanho dos bitmaps consistentes em todas as três plataformas, e isso exige um mergulho mais fundo no assunto.