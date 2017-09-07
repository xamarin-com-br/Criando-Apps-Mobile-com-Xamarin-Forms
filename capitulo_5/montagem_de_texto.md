## Montagem de texto {#montagem-de-texto}

Outra abordagem para a montagem de texto dentro de um retângulo, de um tamanho particular envolve determinar empiricamente o tamanho do texto processado com base em um tamanho de fonte particular e, em seguida, ajustando-se ao tamanho da fonte ou para baixo. Esta abordagem tem a vantagem de trabalhar em dispositivos Android, independentemente de como o usuário definiu a configuração de tamanho da fonte.

Porém, o processo pode ser complicado: O primeiro problema é que não há uma relação linear clara entre o tamanho da fonte e a altura do texto processado. Como o texto torna-se maior em relação à largura do seu recipiente, as linhas de ruptura com maior frequência entre palavras, e mais espaço desperdiçado. Um cálculo para encontrar o tamanho da fonte muitas vezes envolve um loop.

Um segundo problema envolve o mecanismo real de se obter o tamanho de uma Label processado com um tamanho de fonte em particular. Você pode definir um SizeChanged no Label, mas dentro desse manipulador você não quer fazer quaisquer alterações (como a definição da FontSize property) que farão com que chamadas recursivas.

Uma abordagem melhor é chamar o método GetSizeRequest definido por VisualElement e herdada por Label e todos os outros pontos da view. GetSizeRequest requer dois argumentos de uma restrição de largura e uma restrição de altura. Estes valores indicam o tamanho do retângulo em que você quer se ajustar ao elemento, e um ou outro pode ser infinito. Ao usar GetSizeRequest com um Label, geralmente você definir o argumento restrição de largura para a largura do recipiente e define o pedido da altura para to Double.PositiveInfinity.

O método GetSizeRequest retorna um valor do tipo SizeRequest, uma estrutura com duas propriedades nomeadas mínimo e Request, ambos do tipo Size. A propriedade Request indica o tamanho do texto processado. (mais informações sobre este e métodos relacionados aparecer nos próximos capítulos sobre modos de exibição personalizados e layouts.)

O projeto EmpiricalFontSize demonstra essa técnica. Por conveniência, ela define uma estrutura pequena chamada FontCalc que faz a chamada para GetSizeRequest para uma Label particular (já inicializada com texto), um tamanho de fonte, e uma largura do texto:

A altura resultante Label é salvo na propriedade TextHeight.

Quando você faz uma chamada para GetSizeRequest em uma página ou um layout, a página ou o layout deve obter os tamanhos de todos os seus filhos através da árvore view. Isto tem uma penalidade de desempenho, é claro, assim que você deve evitar fazer chamadas, a menos que necessário. Mas um Label não tem filhos, então chamando GetSizeRequest em um Label não é tão ruim. No entanto, você ainda deve tentar otimizar as chamadas. Evite loops através de para determinar valores máximos da fonte que não resulta em texto exceder a altura do recipiente. Um processo que se estreita em algoritmos em um valor ideal é melhor.

GetSizeRequest requer que o elemento deve ser parte de uma árvore view e que o processo de disposição tenha pelo menos parcialmente começado. Não chame GetSizeRequest no construtor de sua classe de página. Você não terá informações a partir dele. A primeira oportunidade razoável é em uma substituição do método OnAppearing da página. Claro, você pode não ter informações suficientes neste momento para passar argumentos para o método GetSizeRequest.

A classe EmpiricalFontSizePage instancia valores FontCalc no SizeChanged do ContentView que hospeda o Label. (Este é o mesmo manipulador de eventos usados no programa EstimatedFontSize.) O construtor de cada valor FontCalc faz GetSizeRequest instanciar um Label e salva o resultado no TextHeight. O SizeChanged começa com ensaios de tamanhos de fonte de 10 e 100 sob o pressuposto de que o valor ideal está em algum lugar entre esses dois e que estes representam limites inferiores e superiores. Daí os nomes de variáveis lowerFontCalc e upperFontCalc:

Em cada iteração do circuito de tempo, as propriedades FontSize desses dois valores FontCalc são calculados e um novo FontCalc é obtido. Isto torna-se o novo valor lowerFontCalc ou upperFontCalc dependendo da altura do texto processado. O loop termina quando o tamanho da fonte é calculado dentro de uma unidade do valor ótimo.

Cerca de sete iterações do ciclo são suficientes para obter um valor que é claramente melhor do que o valor estimado calculado no programa anterior:

Rodar os lados do telefone desencadeia outro novo cálculo que resulta em um tamanho semelhante (embora não necessariamente o mesmo) o de fonte:

Pode parecer que o algoritmo pode ser melhorado. Mas a relação entre o tamanho da fonte e altura texto processado é bastante complexo e, às vezes a abordagem mais fácil é tão bom.