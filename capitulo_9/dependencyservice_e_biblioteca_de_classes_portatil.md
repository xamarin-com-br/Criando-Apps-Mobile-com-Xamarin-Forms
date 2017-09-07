## DependencyService e Biblioteca de Classes Portátil. {#dependencyservice-e-biblioteca-de-classes-port-til}

A técnica ilustrada no programa DisplayPlatformInfoSap2, pode ser implementado em uma solução com uma biblioteca de classes portátil? De primeira, ele não parece ser possível. Apesar de projetos de aplicativos fazer chamadas para bibliotecas o tempo todo, bibliotecas geralmente não podem fazer chamadas para aplicações exceto no contexto de eventos ou funções de retorno de chamada. A classe portátil é fornecida com uma versão do .NET independente de dispositivo, capaz apenas de executar o código entre si ou em outras classes portátil referenciada.

Mas espere: Quando um aplicativo Xamarin.Forms está sendo executado, ele pode usar reflexão .NET para obter acesso à sua própria composição e todas as outras composições no programa. Isto significa que o código no compartilhamento do projeto pode usar reflexão para acessar as classes que existem no conjunto de plataforma a partir de qual classe portátil é referenciado. Essas classes devem ser definidas como público e claro, mas isso é apenas a única exigência.

Antes de começar a escrever o código que explora essa técnica, você deve saber que esta solução já existe sob a forma de uma classe Xamarin.Forms chamado DependencyService. Essa classe usa reflexão .NET para pesquisar todas os outros conjuntos na aplicação, incluindo o conjunto de plataforma especial em si, e fornecer acesso ao código específico da plataforma.

O uso de DependencyService é ilustrado na solução DisplayPlatformInfo, que utiliza uma biblioteca de classes portátil para o código compartilhado. Você começa o processo de utilização DependencyService através da definição de um tipo de interface no projeto de classe portátil que declara as assinaturas dos métodos que você deseja implementar nos projetos de plataformas. Aqui está IPlatformInfo:

Você viu aqueles dois métodos antes. Eles são os mesmos dois métodos implementados nas classes PlatformInfo nos projetos de plataformas em DisplayPlatformInfoSap2.

De um modo muito semelhante ao DisplayPlatformInfoSap2, todos os três projetos de plataformas em DisplayPlatformInfo deve agora ter uma classe que implementa a interface IPlatformInfo. Aqui está a classe no projeto iOS, chamado PlatformInfo:

Esta classe não é acessada diretamente pela classe portátil, por isso o nome do namespace pode ser um nome qualquer. Aqui ele está definido para o mesmo namespace como o outro código no projeto iOS. O nome da classe também pode ser um nome qualquer. Seja qual for o nome dela, no entanto, a classe deve explicitamente implementar a interface IPlatformInfo definido na classe portátil:

Além disso, esta classe deve ser referenciada em um atributo especial fora do bloco de namespace. Você vai vê-lo perto do topo do arquivo seguindo as diretivas using:

A classe DependencyAttribute que define este atributo de dependência é parte de Xamarin.Forms e utilizado especificamente em conexão com DependencyService. O argumento é um objeto Type de uma classe no projeto da plataforma que está disponível para acesso nas classes portáteis. Neste caso, é esta classe PlatformInfo. Esse atributo está ligado ao próprio conjunto de plataforma, de modo que o código em execução nas classes portáteis não tem que procurar em toda a biblioteca para encontrá-lo.

Aqui está a versão Android do PlatformInfo:

E aqui está a um para Windows Phone:

Código do PCL pode então ter acesso a implementação da plataforma privada de IPlatformInfo usando a classe DependencyService. Esta é uma classe estática com três métodos públicos, o mais importante dos quais é nomeado Get. Get é um método genérico cujo argumento é a interface que você definiu, neste caso IPlatformInfo.

O método Get retorna uma instância da classe específico da plataforma que implementa a interface IPlatformInfo. Você pode usar esse objeto para fazer chamadas específicas da plataforma. Isso é demonstrado no arquivo code-behind para o projeto DisplayPlatformInfo:

DependencyService armazena em cache as instâncias dos objetos que ele obtém através do método GET. Isso acelera usos subsequentes Get e também permite que as implementações da plataforma da interface para manter o estado: os campos e propriedades nas implementações da plataforma serão preservadas através de múltiplas chamadas GET. Essas classes também podem incluir eventos ou implementar métodos de retorno de chamada.

DependencyService requer apenas um pouco mais de sobrecarga do que a abordagem mostrada no projeto DisplayPlatformInfoSap2 e é um pouco mais estruturado porque as classes de plataformas individuais implementam uma interface definida no código compartilhado.

DependencyService não é a única maneira de implementar chamadas específicas da plataforma em uma classe portátil. Desenvolvedores aventureiros podem querer usar técnicas dependencyinjection para configurar as classes portáteis para fazer chamadas em outras plataformas. Mas DependencyService é muito fácil de usar, e elimina a maioria das razões para usar um projeto de recurso compartilhado em um aplicativo Xamarin.Forms.

**Plataforma especifica de renderização de som.**

Agora, vamos para o verdadeiro objetivo deste capítulo: dar som para MonkeyTap. Todas as três plataformas de suporte a APIs permitem que um programa possa gerar e jogar ondas sonoras dinamicamente. Esta é a abordagem adotada pelo programa MonkeyTapWithSound.

Ficheiros de música comerciais são frequentemente comprimidas em formatos como MP3\. Mas quando um programa algoritmicamente gera formas de onda, um formato não comprimido é muito mais conveniente. A técnica mais básica é suportada por todas as três plataformas e é chamado de modulação de código de pulso ou PCM. Apesar do nome fantasia, é bastante simples, e é a técnica utilizada para armazenamento de som em CDs de música.

Uma forma de onda PCM é descrito por uma série de amostras a uma taxa constante, conhecida como a frequência de amostragem. CDs de música usam uma taxa normal de 44.100 amostras por segundo. Os arquivos de áudio gerados por programas de computador, muitas vezes usam uma taxa de amostragem de metade desses (22.050) ou um quarto (11.025) quando não se é necessária alta qualidade de áudio. A frequência mais elevada que pode ser gravado e reproduzido é metade da taxa de amostragem.

Cada amostra é um tamanho fixo que define a amplitude da forma de onda naquele ponto no tempo. As amostras de um CD de música estão inscritas em valores de 16 bits. Amostras de 8 bits são comuns quando a qualidade do som não importa tanto. Alguns ambientes apoiam valores de ponto flutuante. Várias amostras podem ser estéreos ou qualquer número de canais. Para efeitos de som simples em dispositivos móveis, som mono geralmente é bom.

O algoritmo de geração de som em MonkeyTapWithSound é codificado para “mono” amostras de 16 bits, mas a taxa de amostragem é definido por uma constante e pode ser facilmente alterada.

Agora que você sabe como DependencyService funciona, vamos examinar o código adicionado ao MonkeyTap para transformá-lo em MonkeyTapWithSound, e vamos olhar para ele de cima para baixo. Para evitar reproduzir um monte de código, o novo projeto contém links para os arquivos MonkeyTap.xaml e MonkeyTap.xaml.cs no projeto MonkeyTap.

No Visual Studio, você pode adicionar itens a projetos como ligações a arquivos existentes, selecionando “**Add &gt; Existing Item”** no menu projeto. Em seguida, use o diálogo “**Add Existing Item”** para navegar para o arquivo. Escolha “**Add as Link”** no menu drop-down no botão “**Add”**.

Em Xamarin Studio, selecione “**Add File to Folder”** arquivo no menu projeto. Depois de abrir o arquivo ou arquivos, um “**Add File to Folder”** caixa de alerta aparece. Escolha “**Add a link to the file”** para o arquivo.

No entanto, depois de seguir estes passos no Visual Studio, também foi necessário editar manualmente o arquivo MonkeyTapWithSound.csproj para alterar o arquivo MonkeyTapPage.xaml a um EmbeddedResource e o Gerador de MSBuild: UpdateDesignTimeXaml. Além disso, uma tag DependentUpon foi adicionada ao arquivo MonkeyTapPage.xaml.cs para referenciar o arquivo MonkeyTapPage.xaml. Isso faz com que o arquivo code-behind para ser identado sob o arquivo XAML na lista de arquivos.

A classe MonkeyTapWithSoundPage então deriva da classe MonkeyTapPage. Embora a classe MonkeyTapPage seja definido por um arquivo XAML e um arquivo code-behind, MonkeyTapWithSoundPage é apenas o código. Quando uma classe é derivada desta forma, manipuladores de eventos no arquivo code-behind original para eventos no arquivo XAML deve ser definida como protegida, e este é o caso.

A classe MonkeyTap também definiu um flashDuration constante como protegida, e dois métodos foram definidos como protected e virtual. O MonkeyTapWithSoundPage substitui estes dois métodos para chamar um método estático chamado SoundPlayer.PlaySound:

O método SoundPlayer.PlaySound aceita uma frequência e uma duração em milissegundos. Todo o resto, o volume, a composição harmônica do som, e como o som é gerado é de responsabilidade do método PlaySound. No entanto, este código faz uma suposição implícita de que SoundPlayer.PlaySound retorna imediatamente e não espera o som para completar de jogo. Felizmente, todas as três plataformas oferecem suporte a APIs de geração de som que se comportam dessa maneira.

A classe SoundPlayer com o método estático PlaySound é parte do projeto MonkeyTapWithSound com biblioteca compartilhada. A responsabilidade deste método é definir uma matriz de dados PCM para o som.

O tamanho desta matriz baseia-se na taxa de amostragem e a duração. Para o laço calcula amostras que definem uma onda triângulo da freqüência solicitado:

Embora as amostras sejam inteiras de 16 bits, duas das plataformas precisam dos dados na forma de uma matriz de bytes, uma conversão ocorre perto do final com Buffer.BlockCopy. A última linha do método usa DependencyService para passar esta matriz de bytes com a taxa de amostragem para as plataformas individuais.

O método DependencyService.Get faz referência a interface IPlatformSoundPlayer que de-multas a assinatura do método PlaySound:

Agora vem a parte difícil: escrever este método PlaySound para as três plataformas!

A versão iOS usa AVAudioPlayer, o que requer dados que inclui o cabeçalho usado em Waveform Audio File Format (arquivos .wav). O código aqui reúne dados em um MemoryBuffer e depois converte isso para um objeto NSData:

Observe os dois elementos essenciais: PlatformSoundPlayer implementa a interface IPlatformSoundPlayer, e a classe é marcada com o atributo de dependência.

A versão Android usa a classe AudioTrack, e que acaba por ser um pouco mais fácil. No entanto, objetos Audiotrack não podem sobrepor-se, por isso é necessário parar e salvar o objeto anterior e pará-lo antes de começar a jogar a próxima:

Um programa do Windows Phone que usa a API do Silverlight (como programas Xamarin.Forms) tem acesso a funções de som em XNA-a de alto nível interface de código gerenciado para DirectX. O código para usar DynamicSoundEffectInstance é extremamente simples:

No entanto, um pouco mais é requerido. Ao utilizar XNA para gerar som, um projeto do Windows Phone requer outra classe que faz chamadas para FrameworkDispatcher.Update a um ritmo constante:

Uma instância dessa classe deve ser iniciada pelo aplicativo. Um lugar conveniente para fazer isso é no arquivo WindowsPhoneApp.xaml:

Neste ponto, você deve ser capaz de ler e compreender um bom pedaço desse arquivo XAML do Windows Phone!

O uso de DependencyService para executar tarefas específicas da plataforma é muito poderoso, mas esta abordagem é insuficiente quando se trata de elementos de interface do usuário. Se para expandir você precisa um arsenal de pontos de vista que adornam as páginas de seus aplicativos Xamarin.Forms, esse trabalho envolve a criação de prestadores específicos da plataforma, um processo discutido em um capítulo posterior.