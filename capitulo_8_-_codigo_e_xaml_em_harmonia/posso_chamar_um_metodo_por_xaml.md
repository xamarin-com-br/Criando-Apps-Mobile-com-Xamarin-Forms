## Posso chamar um método por XAML? {#posso-chamar-um-m-todo-por-xaml}

Anteriormente, a resposta a esta pergunta era **"Não seja ridículo"**, mas agora podemos qualifica-la **"Sim"**. No entanto, não fique muito animado. Os únicos métodos que você pode chamar em XAML são aqueles que retornam objetos \(ou valores\) do mesmo tipo que a classe \(ou estrutura\) que define o método. Esses métodos devem ser **públicos e estáticos**. Eles são chamados às vezes _**creation**_** **ou _**factory methods**._ Você pode instanciar um elemento em XAML através de uma chamada para um desses métodos, especificando o nome do método que utiliza o atributo** x:FactoryMethod** e seus argumentos, usando o elemento **x:Arguments**.

A estrutura **Color **define sete métodos estáticos que retornam valores de **Color**. Este arquivo XAML faz uso de três deles:

![](/assets/08-03-xelements)![](/assets/08-03-elementes01)

Os dois primeiros métodos estáticos chamados são ambos chamados **Color.FromRgb,** porém os tipos de elementos dentro da _tag_ **x:Arguments** distingui entre argumentos int que variam de 0 a 255 e argumentos double que variam de 0 a 1. O terceiro é o método **Color.FromHsla**, que cria um valor de cor a partir de componentes matiz, saturação, luminosidade, e componentes alphas. Curiosamente, esta é a única maneira de definir um valor de Color a partir de valores de HSL em um arquivo XAML usando a API Xamarin.Forms. Aqui está o resultado:

![](/assets/08-04-telas)

