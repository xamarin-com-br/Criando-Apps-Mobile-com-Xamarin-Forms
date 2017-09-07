## 3\. Incluir preenchimento apenas para IOS (PCL ou SAP) {#3-incluir-preenchimento-apenas-para-ios-pcl-ou-sap}

Sim! A classe de dispositivos estático inclui várias propriedades e métodos que permitem que seu código possa lidar com as diferenças de dispositivos em tempo de execução de uma forma muito simples e direta:

*   A propriedade Device.OS retorna um membro da enumeração TargetPlatform: iOS, Android, WinPhone, ou Outro.
*   A propriedade Device.Idiom retorna um membro da enumeração TargetIdiom: Telefone, Tablet, Desktop ou não suportado. (Embora Xamarin.Forms é principalmente destinado a telefones, certamente você pode experimentar com a implantação de tablets.)

Você pode usar essas duas propriedades em if e else para executar código específico para uma determinada plataforma.

Dois métodos chamados OnPlatform oferece soluções ainda mais elegante:

*   O método estático genérico OnPlatform &lt;T&gt; tem três argumentos do tipo T-o primeiro para iOS, o segundo para o Android, ea terceira para Windows Phone e retorna o argumento para a plataforma de execução.
*   O método estático OnPlatform tem quatro argumentos do tipo de acção (a função de delegado .NET que não tem argumentos e retorna void), também na ordem iOS, Android e Windows Phone, com um quarto para um padrão, e executa o argumento para a plataforma de execução.

A classe de dispositivo tem outros fins também: Ela define um método estático para começar a correr um temporizador e uma outra para executar algum código no thread principal. Este último método é útil quando você está trabalhando com métodos assíncronos e suplementares porque o código que manipula a interface do usuário geralmente pode ser executado apenas no segmento principal da interface do usuário.

Em vez de definir a mesma propriedade Padding em todas as três plataformas, você pode restringir o esforço para apenas o iPhone usando o método genérico Device.OnPlatform:

O primeiro argumento é Espessura para iOS, o segundo é para o Android, e a terceira é para o Windows Phone. Especificando explicitamente o tipo dos argumentos Device.OnPlatform dentro dos suportes de ângulo não é necessária se o compilador pode descobrir a partir dos argumentos, assim que isso funciona bem:

Ou, você pode ter apenas um construtor de espessura e usar Device.OnPlatform para o segundo argumento:

Isto é como o Preenchimento geralmente será definido nos programas que se seguem quando é exigido. Claro, você pode substituir alguns outros números para 0s se você quer um pouco de preenchimento adicional na página. Às vezes, um pouco acima dos lados traz uma exibição mais atraente.

No entanto, se você só precisa definir padding para iOS, você pode usar a versão de Device.OnPlatform com argumentos de ação. Estes argumentos são nulos por padrão, então você pode apenas definir o primeiro de uma ação a ser realizada no iOS:

Agora, a instrução para definir o preenchimento é executada somente quando o programa está sendo executado no iOS. Claro que, com apenas um argumento para Device.OnPlatform, poderia ser um pouco obscuro para as pessoas que precisam ler o seu código, de modo que você pode querer incluir o nome do parâmetro anterior ao argumento para torná-lo explícito que esta instrução é executada apenas para iOS:

Nomeando o argumento como esse é um recurso introduzido no C# 4.0.

O método Device.OnPlatform é muito útil e tem a vantagem de trabalhar em ambos os projetos PCL e SAP. No entanto, não se pode aceder APIs dentro das plataformas individuais. Para isso você vai precisar da DependencyService, que é discutido no Capítulo 9.