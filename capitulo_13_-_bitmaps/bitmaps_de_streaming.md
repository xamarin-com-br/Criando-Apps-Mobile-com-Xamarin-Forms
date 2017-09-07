## Bitmaps de streaming {#bitmaps-de-streaming}

Se a classe ImageSource não têm métodos FromUri ou FromResource, você ainda seria capaz de acessar bitmaps através da web ou armazenados como recursos no PCL. Você pode fazer ambos os empregos de, bem como vários outros, com ImageSource.FromStream ou a classe StreamImageSource.

O método ImageSource.FromStream é um pouco mais fácil de usar do que StreamImageSource, mas ambos são um pouco estranhos. O argumento para ImageSource.FromStream não é um objeto de fluxo, mas um objeto Func (um método sem argumentos) que retorna um objeto Stream. A propriedade Stream of racionalizados ImageSource também não é um objeto Stream, mas um objeto Func que tem um gument ar- CancellationToken e retorna um objeto Task &lt;Corrente&gt;.