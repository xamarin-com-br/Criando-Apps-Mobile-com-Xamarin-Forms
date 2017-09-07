## O atributo x:Name {#o-atributo-x-name}

Na maioria das aplicações reais, o arquivo _code-behind_ precisa referenciar elementos definidos no arquivo XAML. Você viu uma maneira de fazer isso no programa **CodePlusXaml** no capítulo anterior: Se o arquivo de code-behind tem conhecimento do layout da árvore visual definida no arquivo XAML, ele pode começar a partir do elemento raiz (a própria página) e localizar elementos específicos dentro da árvore. Este processo é chamado de &quot;_walking the tree_&quot; e pode ser útil para localizar determinados elementos em uma página.

Geralmente, a melhor abordagem é dar aos elementos no arquivo XAML um nome semelhante a um nome de variável. Para isso você faz usa de um atributo que é intrínseco ao XAML, chamado Name. Como o prefixo x é quase universal e usado para atributos intrínsecos ao XAML, esse atributo Nameé comumente referido comox:Name.

O projeto **XamlClock** demonstra o uso de x:Nome. Segue o arquivo XamlClockPage.xaml contendo 2 controles Label, chamado timeLabele dateLabel_._

As regras para x:Name são as mesmas que para os nomes de variáveis em C#. (Você descobrirá o porquê em breve.) O nome deve começar com uma letra ou sublinhado e conter apenas letras, sublinhados e números.

Similarmente ao programa de relógio no Capítulo 5, **XamlClock** usa Device.StartTimerpara disparar um evento periódico para atualizar a hora e a data. Aqui está o arquivo _code-behind_ de XamlClockPage:

Este método de retorno de chamada do timer é chamado uma vez por segundo. O método deve retornar true para continuar o timer. Se ela retorna false, o temporizador para e deve ser reiniciado com outra chamada para _Device.Start-Timer_.O método de retorno referência o timeLabele dateLabelcomo se fossem variáveis ​​normais e define as propriedades de texto de cada um:

Este pode não ser um relógio visualmente impressionante, mas é, definitivamente, funcional.

Como é que o arquivo _code-behind_ pode referenciar os elementos identificados com x:Name? Mágica? Claro que não. O mecanismo é muito evidente quando você examinar o arquivo XamlClockPage.xaml.g.cs que o parser XAML gera a partir do arquivo XAML enquanto o projeto está a sendo construído:

Durante o tempo de compilação, enquanto o _parser_ XAML revira o arquivo XAML, cada atributo x:Name torna-se um campo privado neste arquivo de código gerado. Isso permite que o código no arquivo _code-behind_ referencie esses nomes como se fossem campos normais - que eles definitivamente são. No entanto, os campos são inicialmente null. Somente quando InitializeComponenté chamado, em tempo de execução, os dois campos são definidos através do método FindByName, que é definido na classe NameScopeExtensions. Se o construtor de seu arquivo _code-behind_ tentar fazer referência a estes dois campos antes da chamada InitializeComponent, eles terão valores null.

Este arquivo de código gerado também implica outra regra para valores x:Name que agora é muito óbvio, mas raramente estabelecido explicitamente: os nomes não podem duplicar campos ou propriedade de nomes definidas no arquivo _code-behind_.

Uma vez que estes são campos privados, eles só podem ser acessados ​​a partir do arquivo _code-behind_ e não de outras classes. Se um derivado de ContentPageprecisa expor campos públicos ou propriedades para outras classes, você deve definir neles mesmo.

Obviamente, valores de x:Name deve ser único dentro de uma página XAML. Isto às vezes, pode ser um problema se você estiver usando OnPlatformpara elementos específicos da plataforma no arquivo XAML. Por exemplo, aqui está um arquivo XAML que expressa as propriedades iOS, _Android_, e WinPhonede OnPlatformcomo elementos de propriedade para selecionar um dos três modos de exibição do Label:

O código XAML a seguir não funciona!

Isso não vai funcionar porque vários elementos não podem ter o mesmo nome.

Você poderia dar-lhes nomes diferentes e lidar com os três nomes no arquivo _code-behind_ usando Device.OnPlatform, mas uma solução melhor é manter as especificidades de plataforma tão pequenas quanto possível. Segue a versão do programa **PlatformSpecificLabels** que está incluído com o código de exemplo para este capítulo. Ele tem uma única etiqueta, e tudo é independente de plataforma, exceto para a propriedade Text_:_

E o resultado:

A propriedade de Text é a propriedade de conteúdo do Label, de modo que você não precisa da _tag_ Label.Text como no exemplo anterior. Isso funciona bem: