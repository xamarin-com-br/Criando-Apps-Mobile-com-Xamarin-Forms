## Lidar com tamanhos {#lidar-com-tamanhos}

Algumas referências de tamanhos em conexão com vários elementos visuais:

*   **A barra de status iOS tem uma altura de 20, que você pode ajustar na página.**
*   **O BoxView padrão solicitado de largura e altura é 40.**
*   **O preenchimento padrão dentro de uma moldura é de 20.**
*   **A propriedade padrão de espaçamento sobre a StackLayout é 6.**

O comando Device.GetNamedSize, para vários membros da enumeração NamedSize retorna um número dependente de plataforma apropriada para valores FontSize para uma Label ou Button.

Quais são esses números? Quais são as suas unidades? E como nós inteligentemente definimos propriedades que exigem tamanhos e outros valores?

Boas perguntas. Tamanhos também afetam a exibição do texto. Como você viu, as três plataformas exibem uma quantidade diferente de texto na tela. É a quantidade de texto algo que o Xamarin.Forms application pode prever ou controlar? E mesmo sendo possível, é uma prática de programação correta? Caso um aplicativo ajustar tamanhos de fontes para conseguir uma densidade de texto desejado na tela?

Em geral, quando a programação de um aplicativo Xamarin.Forms, é melhor não ficar muito perto das dimensões numéricas atuais de objetos visuais. É preferível confiar no Xamarin.Forms e para fazer a escolha padrão das três formas de plataformas.

No entanto, há momentos em que um programador precisa saber algo sobre o tamanho de determinados objetos visuais, e o tamanho da tela em que aparecem.

Como você sabe, monitores de vídeo consistem de uma matriz retangular de pixels. Qualquer objeto exibido na tela também tem um tamanho de pixel. Nos primeiros computadores pessoais, os programadores dimensionam e posicionam objetos visuais em unidades de pixels. Mas, com uma maior variedade de tamanhos de tela e densidades de pixel tornou-se inviável trabalhar com pixels, para programadores que tentam escrever aplicações que se parecem em diversos dispositivos. Outra solução foi necessária.