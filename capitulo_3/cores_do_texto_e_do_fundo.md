## Cores do texto e do fundo {#cores-do-texto-e-do-fundo}

Como você viu, o **Label **mostra o texto em uma cor apropriada para o dispositivo. Você pode substituir este comportamento definindo duas propriedades, chamadas de **TextColor **e **BackgroundColor**. O Label autodefine o **TextColor**, mas ele herda o **BackgroundColor** do **VisualElement**, o que significa que a **Page **e o **Layout **também têm uma propriedade **BackgroundColor**.

Você define o **TextColor** e o **BackgroundColor **com um valor do tipo **Color**, que é uma estrutura que define 17 campos estáticos para obtenção de cores comuns. Você pode experimentar o uso destas propriedades com o programa **Greetings** do capítulo anterior. Aqui estão duas delas usadas em conjunto com **HorizontalTextAlignment **and **VerticalTextAlignment **to center the text:

O resultado pode surpreender você. Como as imagens a seguir ilustram, o Label realmente ocupa toda a área da página \(incluindo embaixo da barra de status do iOS\), e as propriedades XAlign e YAlign posicionam o texto dentro desta área:

Aqui está um código onde as cores do texto são as mesmas, porém centraliza o texto usando as propriedades HorizontalOptions e VeriticalOptions:

Agora o Label ocupa apenas o espaço necessário para o texto, e é posicionado no centro da página:

O valor padrão de HorizontalOptions e VerticalOptions não é LayoutOptions.Start, como a aparência padrão do texto poderia sugerir. Em vez disso, o valor padrão é LayoutOptions.Fill. Este é o ajuste que faz com que o Label preencha toda a página. O valor padrão de XAlign e YAlign é TextAlignment.Start, que fez com que o texto fosse posicionado no canto superior esquerdo na primeira versão do programa **Greetings** no capítulo anterior.

Você pode se perguntar: quais são os valores padrões das propriedades TextColor e BackgroundColor, já que os valores padrões resultam em cores diferentes para as diferentes plataformas?

O valor padrão de TextColor e BackgroundColor é na verdade um valor de cor especial chamado de Color.Default, o qual não representa uma cor real, mas em vez disso é usada para referenciar a cor do texto e do fundo apropriada para a plataforma específica. Vamos explorar a cor em mais detalhes.

