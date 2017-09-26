## O atributo x:Name {#o-atributo-x-name}

Na maioria das aplicações reais, o arquivo _**code-behind**_ precisa referenciar elementos definidos no arquivo XAML. Você viu uma maneira de fazer isso no programa **CodePlusXaml** no capítulo anterior: Se o arquivo de **code-behind **tem conhecimento do layout da árvore visual definida no arquivo XAML, ele pode começar a partir do elemento raiz \(a própria página\) e localizar elementos específicos dentro da árvore. Este processo é chamado de "_**walking the tree**_" e pode ser útil para localizar determinados elementos em uma página.

Geralmente, a melhor abordagem é dar aos elementos no arquivo XAML um nome semelhante a um nome de variável. Para isso você faz usa de um atributo que é intrínseco ao XAML, chamado **Name**. Como o prefixo **x** é quase universal e usado para atributos intrínsecos ao XAML, esse atributo **Name **é comumente referido como **x:Name**.

O projeto **XamlClock** demonstra o uso de **x:Nome**. Segue o arquivo **XamlClockPage.xaml** contendo 2 controles **Label**, chamado **timeLabele e dateLabel**_._

![](/assets/08-10-xamlclock)![](/assets/08-10-xamlclock2)As regras para** x:Name** são as mesmas que para os nomes de variáveis em C\#. \(Você descobrirá o porquê em breve.\) O nome deve começar com uma **letra ou sublinhado e conter apenas letras, sublinhados e números**.

Similarmente ao programa de relógio no Capítulo 5, **XamlClock** usa **Device.StartTimer **para disparar um evento periódico para atualizar a hora e a data. Aqui está o arquivo _code-behind_ de **XamlClockPage**:![](/assets/08-11-XamlClockpage)

Este método de retorno de chamada do **timer **é chamado uma vez por segundo. O método deve retornar **true **para continuar o **timer**. Se ela retorna false, o temporizador para e deve ser reiniciado com outra chamada para _**Device.Start-Timer**_**.**O método de retorno referência o **timeLabele dateLabel **como se fossem variáveis ​​normais e define as propriedades de texto de cada um:![](/assets/08-12-telas)

Este pode não ser um relógio visualmente impressionante, mas é, definitivamente, funcional.

Como é que o arquivo _code-behind_ pode referenciar os elementos identificados com **x:Name**? Mágica? Claro que não. O mecanismo é muito evidente quando você examinar o arquivo **XamlClockPage.xaml.g.cs** que o parser XAML gera a partir do arquivo XAML enquanto o projeto está a sendo construído:![](/assets/08-13-genereated)![](/assets/08-13-generates2)Durante o tempo de compilação, enquanto o _**parser**_** **XAML revira o arquivo XAML, cada atributo** x:Name** torna-se **um campo privado** neste arquivo de código gerado. Isso permite que o código no arquivo _**code-behind**_ referencie esses nomes como se fossem campos normais - que eles definitivamente são. No entanto, os campos são inicialmente **null**. Somente quando **InitializeComponent **é chamado, em tempo de execução, os dois campos são definidos através do método **FindByName**, que é definido na classe **NameScopeExtensions**. Se o construtor de seu arquivo _code-behind_ tentar fazer referência a estes dois campos antes da chamada InitializeComponent, eles terão valores **null**.

Este arquivo de código gerado também implica outra regra para valores** x:Name **que agora é muito óbvio, mas raramente estabelecido explicitamente: **os nomes não podem duplicar campos ou propriedade de nomes definidas no arquivo **_**code-behind**_**.**

Uma vez que estes são campos **privados**, eles só podem ser acessados ​​a partir do arquivo _**code-behind**_ e não de outras classes. Se um derivado de **ContentPageprecisa **expor campos públicos ou propriedades para outras classes, você deve definir neles mesmo.

Obviamente, valores de **x:Name **deve ser **único **dentro de uma página XAML. Isto às vezes, pode ser um problema se você estiver usando **OnPlatform **para elementos específicos da plataforma no arquivo XAML. Por exemplo, aqui está um arquivo XAML que expressa as propriedades iOS, _Android_, e WinPhonede **OnPlatform **como elementos de propriedade para selecionar um dos três modos de exibição do Label:![](/assets/08-11-onplataform)![](/assets/08-11-onplatafgorm2)O atributo **x: TypeArguments **do **OnPlatform **deve corresponder exatamente ao tipo de propriedade do destino. Este elemento **OnPlatform **está implícito sendo configurado para a propriedade **Content **do **ContentPage**, e essa propriedade **Content **é do tipo **View**, portanto, o atributo** x: TypeArguments** do **OnPlatform **deve especificar View. No entanto, as propriedades do **OnPlatform **podem ser definidas para qualquer classe que derive desse tipo.

Os objetos definidos para as propriedades iOS, Android e WinPhone podem, de fato, ser diferentes tipos, desde que todos eles derivem da View.

Embora esse arquivo XAML funcione, não é exatamente otimizado. Todas as três exibições de rótulos são instanciadas e inicializadas, mas apenas uma está definida para a propriedade Conteúdo do ContentPage. O problema com esta abordagem surge se você precisar se referir ao rótulo do arquivo de código abaixo e você dá a cada um deles o mesmo nome, assim:

O código XAML a seguir não funciona!![](/assets/08-13-naofunciona)Isso não vai funcionar porque vários elementos não podem ter o mesmo nome.

Você poderia dar-lhes nomes diferentes e lidar com os três nomes no arquivo _**code-behind**_** **usando** Device.OnPlatform**, mas uma solução melhor é manter as especificidades de plataforma tão pequenas quanto possível. Segue a versão do programa **PlatformSpecificLabels** que está incluído com o código de exemplo para este capítulo. Ele tem uma única etiqueta, e tudo é independente de plataforma, exceto para a propriedade Text_:_

E o resultado:![](/assets/08-15telas1)

A propriedade de Text é a propriedade de conteúdo do Label, de modo que você não precisa da _tag_ Label.Text como no exemplo anterior. Isso funciona bem:

![](/assets/08-16-xaml)![](/assets/08-16-xaml2)

