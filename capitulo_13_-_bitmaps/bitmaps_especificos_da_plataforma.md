## Bitmaps específicos da plataforma {#bitmaps-espec-ficos-da-plataforma}

Como você viu, você pode carregar bitmaps através da web ou do projeto PCL compartilhada. Você também pode carregar bitmaps armazenados como recursos nos projetos de plataformas individuais. As ferramentas para este trabalho são o método estático Image- Source.FromFile e a classe FileImageSource correspondente.

Você provavelmente vai usar este recurso principalmente para bitmaps conectados com elementos de interface do usuário. A propriedade Ícone na MenuItem e ToolBarItem é do tipo FileImageSource. A propriedade de imagem no botão, também é do tipo FileImageSource.

Dois outros usos de FileImageSource não serão discutidos neste capítulo: Page classe define uma propriedade Ícone do tipo FileImageSource e uma propriedade BackgroundImage do tipo string, mas que se presume ser o nome de um bitmap armazenado no projeto da plataforma.

O armazenamento de bitmaps nos projetos de plataformas individuais permite um alto nível de especificidade plataforma. Você pode pensar que você pode obter o mesmo grau de especificidade plataforma armazenando bitmaps para cada forma plataforma no projeto PCL e usando o método Device.OnPlatform ou a classe OnPlatform para selecioná-los. No entanto, como você vai descobrir em breve, todas as três plataformas têm provisões para armazenar mapas de bits de pixels de resolução diferente e, em seguida, acessar automaticamente a um ideal. Você pode tirar proveito desse recurso valioso somente se as próprias plataformas individuais carregar os bitmaps, e este é o caso apenas quando utiliza ImageSource.FromFile e FileImageSource.

Os projetos de plataformas em uma solução Xamarin.Forms recém-criado já contêm vários bitmaps. No projeto iOS, você vai encontrá-los na pasta Recursos. No projeto Android, eles estão em subpastas da pasta Recursos. Nos vários projetos de Windows, eles estão na pasta Ativos e subpastas. Esses bitmaps são ícones de aplicações e telas de apresentação, e você vai querer substituí-los quando você a preparar trazer uma aplicação para o mercado.

Vamos escrever um pequeno projeto chamado PlatformBitmaps que acessa um ícone da aplicação de cada projeto de formulário plataforma e exibe o tamanho processado do elemento de imagem. Se você estiver usando FileImageSource para carregar o bitmap (como este programa faz), você precisa definir a propriedade de arquivo para uma string com o nome do arquivo de bitmap. Quase sempre, você estará usando Device.OnPlatform no código ou OnPlatform em XAML para especificar os três nomes:

Quando você acessa um bitmap armazenado na pasta Recursos do projeto iOS ou a pasta de Recursos (ou subpastas) do projeto Android, não prefaciar o nome do arquivo com um nome de pasta. Essas pastas são os repositórios padrão para bitmaps nessas plataformas. Mas bitmaps pode estar em qualquer lugar no projeto do Windows Phone ou Windows (incluindo a raiz do projeto), para que o nome da pasta (se houver) é necessária.

Em todos os três casos, o ícone padrão é o famoso logotipo Xamarin hexagonal (carinhosamente conhecido como o Xamagon), mas cada plataforma tem diferentes convenções para seu tamanho ícone, assim os tamanhos prestados são diferentes:

Se você começar a explorar os bitmaps de ícone nos projetos iOS e Android, você pode ser um pouco confusos: parece haver vários mapas de bits com os mesmos nomes (ou nomes semelhantes) no iOS e Android projetos.

Por hora de mergulhar mais profundamente no assunto da resolução bitmap.