## Fit e fill {#fit-e-fill}

Se você definir a propriedade BackgroundColor do Image em qualquer parte do código anterior e exemplos XAML, você verá que este Image realmente ocupa toda a área retangular da página. Image possui uma propriedade Aspect que controla como o bitmap é processado dentro deste retângulo. Você define essa propriedade com um membro da enumeração Aspect:

*   _AspectFit_ - o padrão
*   _Fill_ - estende-se sem preservar a proporção
*   _AspectFill_ - preserva a relação de aspecto, mas corta a imagem

A configuração padrão é o membro de enumeração Aspect.AspectFit, o que significa que o bitmap se encaixa em limites de seu recipiente, preservando a proporção do bitmap. Como você já viu, a relação entre as dimensões do bitmap e dimensões do recipiente pode resultar em áreas de fundo na parte superior e inferior ou à direita e à esquerda.

Tente isto no projeto WebBitmapXaml:

Agora, o bitmap é expandido para as dimensões da página. Isso resulta na imagem sendo esticada na vertical, de modo que o carro parece bastante curto e atarracado:

Se você virar o telefone de lado, a imagem é esticada na horizontal, mas o resultado não é tão extremo, porque o formato da imagem é um pouco paisagem para começar.

A terceira opção é AspectFill:

Com esta opção, o bitmap enche totalmente o recipiente, mas a relação de aspecto do bitmap é mantida ao mesmo tempo. A única maneira possível é cortando parte da imagem, e você verá que a imagem é realmente cortada, mas de uma maneira diferente nas três plataformas. No iOS e Android, a imagem é cortada no lado superior e inferior ou esquerda e à direita, deixando apenas a parte central do bitmap visível. Nas plataformas Windows Runtime, a imagem é cortada no lado direito ou na parte inferior, deixando o canto superior esquerdo visível: