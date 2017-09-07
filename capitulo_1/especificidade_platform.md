## Especificidade Platform {#especificidade-platform}

Na seção do arquivo XAML envolvendo a ToolbarItem, você também pode ver uma etiqueta com o nome OnPlat- formulário. Esta é uma das várias técnicas em Xamarin.Forms que permitem a introdução de alguma plataforma espe- especifici- no código de outra forma independente da plataforma ou marcação. Ele é usado aqui, porque cada uma das plataformas separadas tem requisitos um pouco diferentes de formato e tamanho de imagem associados a esses ícones.

Uma instalação semelhante existe no código com a classe de dispositivos. É possível determinar qual plataforma o código está sendo executado em e escolher valores ou objetos com base na plataforma. Por exemplo, você pode especificam os tamanhos de fonte diferentes Ify para cada plataforma ou executar diferentes blocos de código com base na plataforma. Você pode querer deixar o usuário manipular um controle deslizante para selecionar um valor em uma plataforma, mas escolher um número a partir de um conjunto de valores explícitos em outra plataforma.

Em algumas aplicações, as especificidades de plataforma mais profundas pode ser desejado. Por exemplo, suponha que sua aplicaç~ao requer as coordenadas GPS do telefone do usuário. Isso não é algo que a versão inicial de Xamarin.Forms fornece, de modo que você precisa para escrever seu próprio código específico para cada plataforma para obter essa informação.

A classe DependencyService fornece uma maneira de fazer isso de uma forma estruturada. Você definir uma inter- face com os métodos que você precisa (por exemplo, IGetCurrentLocation) e, em seguida, implementar essa interface com uma classe em cada um dos projectos de plataforma. Você pode então chamar os métodos em que a interface do projeto Xamarin.Forms quase tão facilmente como se fosse parte da API.

Como discutido anteriormente, cada um dos Xamarin.Forms padrão objetos, tais visuais como Etiqueta, Button, Switch, e Slider-são suportados por uma classe de renderização nas três bibliotecas Xamarin.Forms.Platform. Cada classe de renderização implementa o objeto específico da plataforma que mapeia para o objeto Xamarin.Forms.

Você pode criar seus próprios objetos visuais personalizados com seus próprios representantes personalizados. O objeto visual personalizado vai no projecto de código comum, e os representantes de costume ir nos pro- jectos de plataforma individual. Para torná-lo um pouco mais fácil, geralmente você vai querer derivar de uma classe existente. Dentro das bibliotecas da plataforma Xamarin.Forms individuais, todos os representantes de correspondentes são classes públicas, e você pode deduzir a partir deles também.

Xamarin.Forms permite que você seja como plataforma independente ou como plataforma específica que você precisa para ser.

O Xamarin.Forms não substitui Xamarin.iOS e Xamarin.Android; em vez disso, ele se integra com eles.