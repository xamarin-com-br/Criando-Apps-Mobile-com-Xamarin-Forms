## BITMAPS INDEPENDENTES DE PLATAFORMA {#bitmaps-independentes-de-plataforma}

Aqui está um código simples de um programa chamado **WebBitmapCode** com uma classe de página que usa _ImageSource.FromUri_ para acessar um bitmap a partir do website Xamarin:

Se o URI passado para ImageSource.FromUri não aponta para um bitmap válido, nenhuma exceção é gerada.

Mesmo este pequeno programa pode ser simplificada. ImageSource define uma conversão implícita de string ou Uri para um objeto ImageSource, para que possa definir a string com o URI diretamente para a Source da Image:

Ou, para torná-lo mais detalhado, você pode definir a propriedade Source do Image a um objeto UriImageSource com sua propriedade Uri definida como um objeto Uri:

A classe UriImageSource pode ser preferível se você quiser controlar o cache de imagens web-based. A classe implementa seu próprio cache que usa uma área de armazenamento privada do aplicativo disponível em cada plataforma. UriImageSource define uma propriedade CachingEnabled que tem um valor padrão de true e uma propriedade CachingValidity do tipo TimeSpan que tem um valor padrão de um dia. Isto significa que, se a imagem está reavaliada dentro de um dia, a imagem armazenada é usada. Você pode desabilitar o cache completamente, definindo CachingEnabled como false, ou você pode alterar o tempo de expiração de armazenamento em cache, definindo a propriedade CachingValidity para outro valor TimeSpan.

Independentemente de qual maneira que você faz, por padrão, o bitmap exibido pela Image na view é ampliada para o tamanho de seu recipiente - o ContentPage neste caso - respeitando a proporção do bitmap:

Este bitmap é quadrado, então áreas em branco aparecem acima e abaixo da imagem. Como você pode mudar o seu telefone ou emulador entre modo retrato e paisagem, o bitmap renderizado pode mudar de tamanho, e você verá um espaço em branco na parte superior e inferior ou esquerda e à direita, onde o bitmap não alcança. Você pode colorir a área usando a propriedade BackgroundColor que o Image herda de VisualElement.

O bitmap referenciado no programa WebBitmapCode é 4.096 pixels quadrado, mas um utilitário é instalado no site da Xamarin que permite o download de um arquivo de bitmap muito menor, especificando o URI da seguinte forma:

Agora, o bitmap baixado é de 25 pixels quadrados, mas é novamente esticado para o tamanho de seu recipiente. Cada plataforma implementa um algoritmo de interpolação numa tentativa para suavizar os pixels da imagem expandida para se ajustar à página:

No entanto, se você agora define HorizontalOptions e VerticalOptions na Image para Center - ou coloca o elemento Image em um StackLayout - este bitmap 25-pixel colapsa em uma imagem muito pequena. Este fenómeno é discutido em mais detalhes mais adiante neste capítulo.

Você também pode instanciar um elemento Image no XAML e carregar um bitmap a partir de uma URL, definindo a propriedade Source diretamente a um endereço web. Aqui está o arquivo XAML a partir do programa WebBitmapXaml:

Uma abordagem mais detalhada envolve instanciar explicitamente um objeto UriImageSource e definindo a propriedade Uri:

Independentemente disso, aqui está como ele olha na tela: