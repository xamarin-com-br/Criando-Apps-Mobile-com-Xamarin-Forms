# Capítulo 13 - Bitmaps {#cap-tulo-13-bitmaps}

Os elementos visuais de uma interface gráfica do usuário podem ser divididos entre elementos utilizados para a apresentação (como texto) e aqueles capazes de interação com o usuário, como buttons, sliders, e list boxes.

Texto é essencial para a apresentação, mas as imagens são muitas vezes tão importantes como uma forma de complementar o texto e transmitir informação crucial. A web, por exemplo, seria inconcebível sem fotos. Estas imagens são muitas vezes sob a forma de matrizes retangulares de elementos de imagem (ou pixels) conhecidas como _bitmaps_.

Assim como uma view nomeada _Label_ exibe texto, uma view nomeada _Image_ exibe bitmaps. Os formatos bitmap suportados pelo iOS, Android e Windows Runtime são um pouco diferentes, mas se você ficar com JPEG, PNG, GIF e BMP em suas aplicações Xamarin.Forms, você provavelmente não sentiria qualquer problema.

_Image_ possui uma propriedade _Source_ onde você define um objeto do tipo _ImageSource_, que referencia o bitmap exibido pela imagem. Bitmaps podem vir de uma variedade de sources, então a classe _ImageSource_ define quatro métodos de criação estáticos que retornam um objeto _ImageSource_:

*   _ImageSource.FromUri_ para acessar um bitmap através da web.
*   _ImageSource.FromResource_ para um bitmap armazenado como um recurso incorporado na aplicação PCL.
*   _ImageSource.FromFile_ para um bitmap armazenado como conteúdo em um projeto de plataforma individual.
*   _ImageSource.FromStream_ para carregar um bitmap usando um objeto .NET _Stream_.

_ImageSource_ também tem três classes descendentes, denominados _UriImageSource, FileImageSource_ e _StreamImageSource_, que você pode usar em vez do primeiro, terceiro e quarto métodos de criação estáticos. Geralmente, os métodos estáticos são mais fáceis de usar no código, mas as classes descendentes algumas vezes são necessárias em XAML.

Em geral, você vai usar os métodos ImageSource.FromUri e _ImageSource.FromResource_ para obter bitmaps independentes de plataforma para fins de apresentação e _ImageSource.FromFile_ para carregar bitmaps específicos da plataforma para objetos de interface do usuário. Bitmaps pequenos desempenham um papel crucial no _MenuItem_ objetos e _ToolbarItem_, e você também pode adicionar um bitmap em um _Button_.

Este capítulo começa com o uso de bitmaps independentes de plataforma obtidos a partir dos métodos _ImageSource.FromUri_ e _ImageSource.FromResource_. Em seguida, explora alguns usos do método _ImageSource.FromStream_. O capítulo termina com o uso de _ImageSource.FromFile_ para obter bitmaps específicos da plataforma para toolbars e buttons.