## Textos apropriados para tamanho de dispositivos {#textos-apropriados-para-tamanho-de-dispositivos}

Talvez seja necessário encaixar um bloco de texto para uma área retangular. É possível calcular um valor para a propriedade FontSize property do Label com base no número de caracteres de texto, o tamanho da área rectangular, com dois números. (No entanto, esta técnica não funciona em Android, a menos que a definição do tamanho da fonte seja Normal.)

O primeiro número é o espaçamento entre linhas. Esta é a altura vertical de uma vista para Label por linha de texto. Para as fontes padrões associados com as três plataformas, é mais ou menos relacionadas com a propriedade FontSize, como segue:

*   **iOS: lineSpacing = 1.2 * label.FontSize**
*   **Android: lineSpacing = 1.2 * label.FontSize**
*   **Windows Phone: lineSpacing = 1.3 * label.FontSize**

O segundo número é a medida da largura do caractere. Para uma mistura normal de letras maiúsculas e minúsculas para as fontes padrões, essa largura média de carácter é cerca da metade do tamanho da fonte, independentemente da plataforma:

*   **averageCharacterWidth = 0.5 * label.FontSize**

Por exemplo, suponha que queira encaixar uma seqüência de texto contendo 80 caracteres em uma largura de 320 unidades, e você gostaria que o tamanho da fonte possa ser tão grande quanto possível. Divida a largura (320) pela metade do número de caracteres (40), e você terá um tamanho de fonte de 8, que pode ser definida com a propriedade FontSize do Label. Para o texto que é um pouco indeterminado e não pode ser testado de antemão, você pode querer fazer este cálculo um pouco mais conservadora para evitar surpresas.

O seguinte programa usa tanto o espaçamento entre linhas e largura de carácter médio para caber um parágrafo do texto na página, ou melhor, na página menos a área na parte superior do iPhone ocupada pela barra de status. Para fazer a exclusão da barra de status estatuto do iOS, é um pouco mais fácil neste programa, o programa usa um ContentView.

ContentView deriva do layout, mas adiciona uma propriedade de conteúdo apenas para o que ele herda de Layout. ContentView é a classe base para moldura, mas não acrescenta muita funcionalidade própria. No entanto, ele pode ser útil para preparar um grupo de pontosa para definir uma nova exibição personalizada e para simular uma margem.

Como você deve ter notado, Xamarin.Forms não tem nenhum conceito de margem, que tradicionalmente é semelhante ao preenchimento, exceto que o preenchimento é dentro de uma visão e de uma parte da view, enquanto a margem é fora da view e, na verdade, parte da view do pai . A ContentView nos permite simular isso. Se você encontrar uma necessidade de definir uma margem em uma view, colocar a view em um ContentView. ContentView e defina a propriedade Padding do Layout.

O programa EstimatedFontSize usa ContentView de uma forma ligeiramente diferente: ele define o preenchimento habitual na página para evitar a barra de status do iOS, mas, em seguida, define um ContentView como o conteúdo dessa página. Assim, este ContentView é do mesmo tamanho que a página, mas excluindo a barra de status iOS. É nesta ContentView que o evento SizeChanged está ligado, e é o tamanho deste ContentView que é usada para calcular o tamanho da fonte de texto.

O SizeChanged usa o primeiro argumento para obter o evento do objeto firing (neste caso, o ContentView), que é o objeto em que a Label tem de encaixar. O cálculo é descrito nos comentários:

Os marcadores de texto chamados &quot;? 1&quot;, &quot;2 ??&quot;, &quot;3?&quot;, E &quot;? 4&quot; foram escolhidos para serem únicos, mas também para serem o mesmo número de caracteres como os números que eles substituem.

Se o objetivo é fazer o texto tão grande quanto possível sem que o texto derrame para fora da página, os resultados validam essa abordagem:

Não é tão ruim assim. O texto, na verdade, exibido no iPhone e Android eem 14 linhas. Não é necessário para o mesmo FontSize ser calculados no modo paisagem, mas isso acontece às vezes: