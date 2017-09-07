## Cliques de Botão {#cliques-de-bot-o}

Os componentes de uma interface gráfica podem ser divididos aproximadamente em componentes que são usados para a apresentação (exibição de informações para o usuário) e interação (obtenção de entrada do usuário). Enquanto o componente Label é o mais básico de apresentação, o componente Button é provavelmente a visão interativa arquetípica. O Button sinaliza um comando. É a maneira do usuário de dizer ao programa para iniciar alguma ação—para fazer alguma coisa.

Um botão Xamarin.Forms exibe o texto, com ou sem uma imagem em anexo. (Somente botões de texto são descritos neste capítulo; acrescentar um botão de imagem é descrito no Capítulo 13, &quot;Bitmaps&quot;.) Quando um dedo pressiona o botão, o botão muda sua aparência um pouco para fornecer resposta ao usuário. Quando o dedo soltá-lo, o botão dispara um evento Clicked. Os dois argumentos do evento Clicked são típicos dos eventos Xamarin.Forms:

*   O primeiro argumento é o objeto que disparou o evento. Para o evento Clicked, este é o próprio componente que foi pressionado.
*   O segundo argumento, em alguns casos, fornece mais informações sobre o evento. Para o evento Clicked, o segundo argumento é simplesmente um objeto EventArgs que não fornece informação adicional.

Quando um usuário começa a interagir com uma aplicação, algumas necessidades especiais surgem: O aplicativo deve fazer um esforço para salvar os resultados dessa interação no caso do programa ser encerrado antes que o usuário tenha terminado de trabalhar com ele. Por esse motivo, este capítulo também descreve como um aplicativo pode salvar os dados transitórios, especialmente no contexto dos eventos de ciclo de vida do aplicativo.