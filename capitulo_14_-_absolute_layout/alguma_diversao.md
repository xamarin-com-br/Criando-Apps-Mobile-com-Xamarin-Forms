## Alguma Diversão {#alguma-divers-o}

Como você provavelmente pode ver até agora, o AbsoluteLayout é muitas vezes usado para algumas finalidades especiais que não seria fácil contrário. Alguns deles podem realmente ser classificada como &quot;diversão&quot;.

**DotMatrixClock** exibe os dígitos da hora atual utilizando a 5 × 7 display dot matrix simulado. Cada ponto é um BoxView, individualmente dimensionados e posicionados na tela e colorido vermelho ou cinza-claro, dependendo se o ponto está ligado ou desligado. É concebível que os pontos de este relógio poderiam ser organizados em elementos StackLayout aninhados ou uma grade, mas cada BoxView precisa ser dado um tamanho de qualquer maneira. A grande quantidade e regularidade desses pontos de vista sugere que o programador sabe melhor do que uma classe de layout como organizá-los na tela, como a classe de layout precisa para executar os cálculos de localização de uma forma mais generalizada. Por essa razão, este é um trabalho ideal para AbsoluteLayout.

Um arquivo XAML define um pouco acima na página e prepara um AbsoluteLayout para o preenchimento por código.

O arquivo code-behind contém vários campos, incluindo duas matrizes, numberPatterns nomeados e colonPattern, que definem os padrões de matriz de pontos para os 10 dígitos e um separador por dois pontos:

Os campos também são definidos por uma matriz de objetos BoxView para os seis dígitos do tempo de dois dígitos cada hora para, minutos e segundos. O número total de pontos na horizontal (definido como horzDots) inclui cinco pontos para cada um dos seis dígitos, quatro pontos para os dois pontos entre as horas e os minutos, para quatro do cólon entre os minutos e os segundos, e uma largura de pontos entre os dígitos de outra maneira.

O construtor do programa (ver abaixo) cria um total de 238 objetos BoxView e os adiciona a uma AbsoluteLayout, mas também poupa o BoxView objetos para os dígitos na matriz digitBoxViews. (Em teoria, os objetos BoxView pode ser referenciado posteriormente pela indexação da coleção Children of the AbsoluteLayout. Mas nessa coleção, eles aparecem simplesmente como uma lista linear. Armazenando-os também em uma matriz multidimensional permite que eles sejam mais facilmente identificados e referenciados. ) Todo o posicionamento e dimensionamento é proporcional com base numa AbsoluteLayout que é assumida para ter uma razão de aspecto de 41 a 7, o qual compreende os 41 BoxView larguras e alturas 7 BoxView.

Como você deve se lembrar, os horzDots e vertDots funções são definidas para 41 e 7, respectivamente. Para encher o AbsoluteLayout, cada BoxView precisa ocupar uma fracção da largura igual a 1 / horzDots e uma fracção da altura igual a 1 / vertDots. A altura e largura definida para cada BoxView é de 85 por cento do referido valor para separar os pontos suficiente de modo que eles não fiquem em se:

Para posicionar cada BoxView, o construtor calcula xIncrement proporcional e valores yIncrement assim:

Os denominadores aqui são 40 e 6 para que o X final e Y coordenadas posicionais são valores de 1.

Os objetos BoxView para os dígitos de tempo não são coloridos em tudo no construtor, mas aqueles para os dois dois pontos são dadas uma propriedade cores com base na matriz colonPattern. O construtor DotMatrixClockPage conclui por um segundo de timer.

O manipulador SizeChanged para a página é definido a partir do arquivo XAML. O AbsoluteLayout é automaticamente esticado horizontalmente para preencher a largura da página (menos o estofamento), de modo que o HeightRequest realmente apenas define a relação de aspecto:

Parece que o manipulador de eventos Device.StartTimer deve ser bastante complexo, pois é responsável por definir a propriedade de cor de cada BoxView com base nos dígitos do tempo atua. No entanto, a semelhança entre as definições da matriz e a matriz numberPatterns digitBoxViews torna surpreendentemente simples:

E aqui está o resultado:

É claro que, quanto maior, melhor, então você provavelmente vai querer ligar o telefone (ou o livro) de lado por algo grande o suficiente para ler a partir do outro lado da sala:

Outro tipo especial de aplicação adequado para AbsoluteLayout é animação. O programa BouncingText usar seu arquivo XAML para instanciar dois elementos do rótulo:

Observe que os atributos AbsoluteLayout.LayoutFlags estão definidas para PositionProportional. A etiqueta calcula seu próprio tamanho, mas o posicionamento é proporcional. Valores entre 0 e 1 pode posicionar os dois elementos do rótulo em qualquer lugar dentro da página.

O arquivo code-behind inicia um temporizador indo com uma duração de 15 milissegundos. Isto é equivalente a aproximadamente 60 carrapatos por segundo, o que é geralmente a taxa de atualização de monitores de vídeo. A duração do temporizador de 15 milissegundos é ideal para a realização de animações:

O manipulador OnTimerTick calcula um tempo decorrido desde que o programa começou e converte isso para um valor t (por tempo) que vai de 0 a 1 a cada dois segundos. O segundo cálculo de t torna aumentar 0-1 e depois diminuir de volta para baixo a 0 a cada dois segundos. Esse valor é passado diretamente para o construtor de retângulo nas duas chamadas AbsoluteLayout.SetLayoutBounds. O resultado é que a primeira etiqueta move-se horizontalmente através do centro da tela e parece saltar fora os lados esquerdo e direito. A segunda etiqueta move verticalmente para cima e para baixo do centro da tela e parece saltar fora a parte superior e inferior:

Os dois pontos de vista do rótulo conhecer brevemente no centro a cada segundo, como a captura de tela do Windows Phone confirma. De agora em diante, as páginas de nossas aplicações Xamarin.Forms vai se tornar mais ativo e animado e dinâmico. No próximo capítulo, você verá como as visualizações interativas de Xamarin.Forms estabelecer um meio de comunicação entre o usuário e o aplicativo.