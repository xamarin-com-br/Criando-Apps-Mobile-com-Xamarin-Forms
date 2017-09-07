## 1\. Incluir preenchimento na página {#1-incluir-preenchimento-na-p-gina}

A classe de página define uma propriedade chamada Padding que marca uma área em torno do perímetro interior da página em que o conteúdo não pode intrometer. A propriedade Padding é do tipo de espessura, uma estrutura que define quatro propriedades nomeadas Esquerda, Cima, Direita, Inferior. (Você pode querer memorizar essa ordem que você vai definir as propriedades no construtor de espessura, bem como em XAML.). A propriedade Espessura também define construtores para definir a mesma quantidade de preenchimento em todos os quatro lados ou para definir a mesma quantidade, à esquerda e à direita e na parte superior e na parte inferior.

Um pouco de pesquisa no seu motor de busca favorito irá revelar que a barra de status iOS tem uma altura de 20\. (Vinte quê? Você pode perguntar. Vinte pixels? Na verdade, não. Por agora, basta pensar neles como 20 &quot;unidades&quot;. Para a maioria dos Xamarin.Forms você não precisa se preocupar com tamanhos numéricos, mas o Capítulo 5, &quot;Lidar com tamanhos&quot;, irá fornecer alguma orientação quando você precisa chegar até o nível de pixel).

Você pode acomodar a barra de status da seguinte forma:

Agora o cumprimento aparece 20 unidades a partir do topo da página:

A definição da propriedade Padding no ContentPage resolve o problema do texto substituindo a barra de status iOS, mas também define o mesmo preenchimento no Android e Windows Phone, onde não é necessária. Existe uma maneira de definir este preenchimento apenas no iPhone?