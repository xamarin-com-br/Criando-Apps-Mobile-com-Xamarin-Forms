## Imagens de botão {#imagens-de-bot-o}

Botão define uma propriedade de imagem do tipo FileImageSource que você pode usar para fornecer uma imagem pequena plemental apoio que é exibido à esquerda do texto do botão. Este recurso não se destina a uma idade somente botão im-; se é isso que você quer, o programa ImageTap neste capítulo é um bom ponto de partida.

Você quer as imagens para ser cerca de um quinto de polegada de tamanho. Isso significa que você quer que eles se render em 32 unidades independentes de dispositivo e mostrar-se contra o fundo do botão. Para iOS eo UWP, isso significa que uma imagem em preto sobre um fundo branco ou transparente. Para Android, Windows 8.1, e Windows Phone 8.1, você vai querer uma imagem em branco contra um fundo transparente.

Todos os bitmaps no projeto ButtonImage são do diretório Barra de ação do Android de- assinar coleção Icons ea 03_rating_good e 03_rating_bad subdiretórios. Estes são &quot;polegares para cima&quot; e &quot;polegares para baixo&quot; imagens.

As imagens são iOS a partir do diretório holo_light (imagens em preto sobre fundos transparentes) com as seguintes conversões de nome de arquivo:

*   drawable-mdpi / ic_action_good.png não renomeado
*   drawable-xhdpi / ic_action_good.png renomeado para ic_action_good@2x.png~~V E da mesma forma para ic_action_bad.png.

As imagens do Android estão a partir do diretório holo_dark (imagens brancas em fundos transparentes) e incluem todos os quatro tamanhos dos subdiretórios drawable-mdpi (32 pixels quadrados), drawable-hdpi (48 pixels), drawable-xhdpi (64 pixels) e drawable -xxhdpi (96 pixels quadrado).

As imagens para os vários projectos Windows Runtime são todos uniformemente o bitmaps de 32 de pixel dos diretórios drawable-mdpi.

Aqui está o arquivo XAML que define a propriedade do ícone por dois elementos de botão:

E aqui estão elas:

Não é muito, mas o bitmap acrescenta um pouco de brio para o botão normalmente somente texto.

Outro uso importante para pequenos bitmaps é o menu de contexto disponível para itens na TableView. Mas um pré-requisito para que é uma exploração profunda das várias vistas que contribuem para a interface interactiva de Xamarin.Forms. Que está chegando no Capítulo 15.

Mas primeiro vamos olhar para uma alternativa para StackLayout que lhe permite vistas criança de posição de uma forma completamente flexível.