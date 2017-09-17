## Diga Olá {#diga-ol}

Usando o Microsoft Visual Studio ou Xamarin Studio, vamos criar um novo aplicativo Xamarin.Forms usando um modelo padrão. Este processo cria uma solução que contém até quatro projetos: três projetos de plataforma – iOS, Android e Windows Phone – e um projeto comum para a maior parte do código do seu aplicativo.

No Visual Studio, selecione a opção do menu **File &gt; New &gt; Project**. À esquerda da caixa de diálogo **New Project**, selecione **Visual C\#** e, em seguida, **Mobile Apps**.

No Xamarin Studio, selecione **File &gt; New &gt; Solution** no menu, e no lado esquerdo da caixa de diálogo **New Solution**, selecione **C\#** e, em seguida, **Mobile Apps**.

Em ambos os casos, a seção central da caixa de diálogo lista três modelos de solução disponíveis:

* **Blank App \(Xamarin.Forms Portable\)**
* **Blank App \(Xamarin.Forms Shared\)**
* **Class Library \(Xamarin.Forms Portable\)**

E agora? Nós definitivamente queremos criar uma solução **Blank App**, ou seja, em branco, mas de que tipo?

O termo "Portable", neste contexto, refere-se a uma Biblioteca de Classes Portátil \(PCL\). Todo o código comum da aplicação torna-se uma biblioteca de vínculo dinâmico \(DLL\) que é referenciado por todas as plataformas individuais do projeto.

O termo "Shared" neste contexto significa um projeto de recurso compartilhado \(SAP\) contendo arquivos de código solto \(e talvez outros arquivos\) que são compartilhados entre os projetos de plataformas, essencialmente tornando-se parte de cada projeto de plataforma.

Por enquanto, escolha o primeiro: **Blank App \(Xamarin.Forms Portable\)**. Selecione um local do disco para a solução, e dê um nome, por exemplo, **Hello**.

Se você estiver executando o Visual Studio, quatro projetos são criados: um projeto comum \(o projeto PCL\) e três projetos de aplicação. Para a solução chamada **Hello**, são eles:

* Um projeto Biblioteca de Classes Portátil chamado **Hello** que é referenciado pelos três aplicativos do projeto;
* Um projeto de aplicativo para o Android, chamado **Hello.Droid**;
* Um projeto de aplicativo para iOS, chamado **Hello.iOS**; e
* Um projeto de aplicativo para Windows Phone, chamado **Hello.WinPhone**.

Se você estiver executando Xamarin Studio no Mac, o projeto do Windows Phone não é criado, e se você estiver executando Xamarin Studio no PC, nem o iOS nem o programa do Windows Phone é criado.

Antes de continuar, certifique-se de que as configurações do projeto estão corretas. No Visual Studio, selecione o item de menu **Build &gt; Configuration Manager**. Na caixa de diálogo **Configuration Manager**, você verá o projeto PCL e os três projetos de aplicação. Certifique-se de que a caixa **Build** está verificada para todos os projetos e a caixa de **Deploy** está verificada para todos os projetos dos aplicativos \(a não ser que a caixa fica acinzentado\). Selecinone na coluna **Platform**: Se o projeto **Hello** estiver na lista, ele deve ser sinalizado como **Any CPU**. O projeto **Hello.Droid** também deve ser marcado como **Any CPU**. \(Para os dois tipos de projetos, **Any CPU** é a única opção.\) Para o projeto **Hello.iOS**, escolha **iPhone** ou **iPhoneSimulator** dependendo de como você vai estar testando o programa. Para o projeto **Hello.WinPhone**, você pode selecionar **x86** se você estiver usando um emulador na tela, **ARM** se você vai estar instalando em um telefone real, ou **Any CPU** para a implantação de um ou outro. Independentemente da sua escolha, Visual Studio gera o mesmo código.

Se um projeto não parecer na compilação ou implantação no Visual Studio, verifique novamente as configurações na caixa de diálogo **Configuration Manager**. Às vezes, uma configuração diferente torna-se ativo e pode não incluir o projeto PCL.

No Xamarin Studio no Mac, você pode alternar entre instalar no iPhone e simulador iPhone através do menu **Project &gt; Active Configuration**.

No Visual Studio, você provavelmente vai querer exibir as barras de ferramentas iOS e Android. Essas barras de ferramentas permitem que você escolha entre os emuladores e dispositivos e permitem que você gerencie os emuladores. No menu principal, certifique-se que **View &gt; Toolbars &gt; iOS** e **Views &gt; Toolbars &gt; Android** estão checados.

Como a solução contém em qualquer lugar de dois a quatro projetos, você deve designar que programa é iniciado quando você optar por executar ou depurar um aplicativo.

No **Solution Explorer** do Visual Studio, clique com o botão direito em qualquer um dos três projetos de aplicação e selecione **Set As StartUp Project** no item do menu. Você pode então selecionar a posicionar ou um emulador ou um dispositivo real. Para criar e executar o programa, selecione o item de menu **Debug&gt; Start Debugging**.

Na **Solution** no Xamarin Studio, clique no ícone de ferramenta pequena que aparece à direita de um projeto selecionado e selecione **Set As StartUp Project** a partir do menu. Você pode então escolher **Run &gt; Start Debugging** no menu principal.

Se tudo correr bem, o esqueleto do aplicativo criado pelo modelo será executado e você verá uma pequena mensagem:

![](/assets/2.1-mensagem.PNG)

Como você pode ver, estas plataformas têm diferentes esquemas de cores. Por padrão, o esquema de cores do Windows Phone é como o esquema de cores Android em que ele exibe o texto claro em um fundo escuro, mas no Windows Phone este esquema de cores é mutável pelo usuário. Mesmo em um emulador do Windows Phone, você pode alterar o esquema de cores na seção **Themes** do aplicativo **Settings** e, em seguida, execute novamente o programa.

O aplicativo não só é executado no dispositivo ou emulador, mas instalado. Ele aparece com as outras aplicações no telefone ou emulador e pode ser executado a partir de lá. Se você não gosta do ícone do aplicativo ou como o nome do aplicativo é exibido, você pode mudar isso nos projetos de plataformas individuais.

