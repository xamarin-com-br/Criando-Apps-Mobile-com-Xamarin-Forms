## Quebra automática de texto nosparágrafos {#quebra-autom-tica-de-texto-nospar-grafos}

Mostrar um parágrafo de texto é tão fácil como mostrar uma linha simples de texto. Apenas crie o texto longo o suficiente para fazer quebra automática de texto em múltiplas linhas:

Note o uso de códigos Unicode embutidos para abertura e fechamento de “aspas duplas” (\u201C e \u201D) e para o travessão ‘em’ (\u2014). A propriedade Padding foi ajustada para 5 unidades ao redor da página para evitar que o texto encoste nas bordas da tela, e a VerticalOptions também foi usada para centralizar verticalmente todo o parágrafo na página:

Para este parágrafo de texto, ajustar a HorizontalOptions mudará todo o parágrafo horizontalmente levemente para a esquerda, centro ou direita. O deslocamento é pequeno porque a largura do parágrafo é a largura da linha de texto mais longa. Já que a quebra de texto é dirigida pela largura da página (menos o Padding), o parágrafo provavelmente ocupa apenas um pouco menos de largura que a largura disponível para ele na página.

Porém, ajustar XAlign tem um efeito muito mais profundo: o ajuste desta propriedade afeta o alinhamento das linhas individualmente. Um ajuste de TextAlignment.Center centralizará todas as linhas do parágrafo, e TextAlignment.Right alinhará todas à direita. Você pode utilizar HorizontalOptions além de XAlign para deslocar todo o parágrafo levemente para o centro ou à direita.

No entanto, depois que você ajustar VerticalOptions para Start, Center, ou End, qualquer ajuste de YAlign não terá efeito.

o Label tem a propriedade LineBreakMode que você pode ajustar para uma das opções da lista enumerada de LineBreakMode do Xamarin.Forms, se você não quiser que o texto quebre automaticamente, ou para selecionar opções de truncamento do texto.

Não há nenhuma propriedade para especificar um recuo da primeira linha para o parágrafo, mas você pode adicionar seu próprio recuo com caracteres de espaço de vários tipos, tal como o código Unicode para espaço (\u2003).

Você pode mostrar vários parágrafos com um simples Label finalizando cada parágrafo com um ou mais caracteres de quebra de linha (\n). No entanto, faz mais sentido usar um Label separado para cada parágrafo, como será demonstrado no Capítulo 4, “Rolando a pilha”.

A classe Label tem muita flexibilidade na formatação. Como você verá em breve, as propriedades definidas por Label permitem que você especifique um tamanho de fonte, texto em negrito ou itálico, e você também pode especificar formatação de texto diferente dentro de um único parágrafo.

O Label também permite especificar cores, e um pouco de experimentação com cores vai demonstrar a profunda diferença entre HorizontalOptions e VerticalOptions e XAlign e YAlign.