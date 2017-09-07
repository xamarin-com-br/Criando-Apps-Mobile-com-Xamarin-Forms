## Um clock para ajustar tamanho {#um-clock-para-ajustar-tamanho}

A classe de dispositivo inclui um método estático StartTimer que permite que você defina um temporizador que dispara um evento periódico. A disponibilidade de um evento timer significa que a aplicação do relógio é possível, mesmo que ele exibe o tempo apenas em texto.

O primeiro argumento de um intervalo Device.StartTimer é expressa como um valor TimeSpan. O cronômetro dispara um evento periodicamente com base nesse intervalo. (Você pode ir para baixo tão baixo em 15 ou 16 milissegundos, que é o período da taxa de quadros, 60 quadros por segundo comum em monitores de vídeo.) O manipulador de eventos não tem argumentos, mas deve retornar true para manter o temporizador indo.

O programa FitToSizeClock cria um Label para exibir a hora e, em seguida, define dois eventos: O Evento SizeChanged para alterar o tamanho da fonte, e o evento Device.StartTimer para intervalos de um segundo para mudar a propriedade de Text. Ambos os eventos simplesmente alteram uma propriedade do Label, e ambos são expressos como funções lambda para que eles possam acessar o Label sem que seja armazenado como campo:

O StartTimer especifica uma cadeia de formatação personalizada para DateTime que resulta em 10 ou 11 caracteres, mas dois deles são letras maiúsculas, e são mais larga. O SizeChanged implicitamente assume que 12 caracteres são exibidos, definindo o tamanho da fonte para um sexto da largura da página:

Naturalmente, o texto é muito maior no modo paisagem:

Mais uma vez, esta técnica funciona em Android apenas se o tamanho da fonte esteja definido para Normal.

O primeiro segundo não vai assinalar exatamente no início de cada segundo, assim que o tempo exibido pode não concordar precisamente com outros mostradores de tempo no mesmo dispositivo. Você pode torná-lo mais preciso, definindo uma escala de timer mais frequente. O desempenho não será afetado muito porque a tela ainda muda apenas uma vez por segundo e não vai exigir um novo ciclo de layout até então.