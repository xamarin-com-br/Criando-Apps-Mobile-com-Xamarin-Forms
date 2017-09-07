## Quais são esses símbolos? {#quais-s-o-esses-s-mbolos}

No Visual Studio, clique com o botão direito do mouse no nome do projeto no **Solution Explorer** e selecione **Properties**. À esquerda da tela de propriedades, selecione **Build**, e olhe para o campo **Conditional compilation symbols**.

No Xamarin Studio, selecione um projeto de aplicativo na lista **Solution**, chame o menu de ferramentas drop-down e selecione **Options**. Na esquerda da caixa de diálogo **Project Options**, selecione **Build &gt; Compiler**, e olhe para o campo **Define Symbols**.

Você descobre que o símbolo __IOS__ está definido para o projeto iOS (que é dois sublinhados antes e depois) e WINDOWS_PHONE está definido para o projeto do Windows Phone. Você não vai ver nada para o Android, mas o __ANDROID__ identificador é definido de qualquer maneira, assim como vários identificadores __ANDROID_nn__, onde nn é cada nível API Android suportado. Seu arquivo de código compartilhado pode incluir blocos como este:

Isso permite que seus arquivos de código compartilhado possam executar as classes específicas da plataforma de código ou de acesso específicos da plataforma, incluindo nos projetos de plataformas individuais. Você também pode definir os seus próprios símbolos condicionais, que você gostaria.

Estas diretivas de pré-processador não fazem sentido em um projeto de Biblioteca de Classes Portátil. O PCL é totalmente independente das três plataformas, e esses identificadores nos projetos da plataforma são ignoradas quando o PCL é compilado.

O conceito do PCL originalmente surgiu porque todas as plataformas que usam .NET, na verdade, usam um pouco diferente o subconjunto de .NET. Se você deseja criar uma biblioteca que pode ser usada entre múltiplas plataformas .NET, você precisa usar apenas as partes comuns desses subconjuntos .NET.

O PCL se destina a ajudar, contendo código que é utilizável em vários (mas específicos) formas da plataforma .NET. Consequentemente, qualquer PCL particular contém algumas bandeiras embutidos que indicam quais plataformas que ele suporta. A PCL usado em um aplicativo Xamarin.Forms deve suportar as seguintes plataformas:

*   **.NET Framework 4.5**
*   **Windows 8**
*   **Windows Phone Silverlight 8**
*   **Xamarin.Android**
*   **Xamarin.iOS**
*   **Xamarin.iOS (Classic)**

Se você precisar de comportamento um específico da plataforma no PCL, você não pode usar as diretivas de pré-processador C# são código que são usandos apenas em tempo de compilação. Você precisa de algo que funcione em tempo de execução, tais como a classe de dispositivos Xamarin.Forms. Você verá em breve um exemplo.

O Xamarin.Forms PCL pode acessar outras PCLs suportando as mesmas plataformas, mas não pode acessar diretamente as classes definidas nos projetos de aplicativos individuais. No entanto, se isso é algo que você precisa fazer e você vai ver um exemplo no capítulo 9\. Xamarin.Forms fornece uma classe chamada Dependency Service que lhe permite acessar o código específico da plataforma do PCL de maneira metódica.

A maioria dos programas neste livro usa a abordagem PCL. Esta é a abordagem recomendada para Xamarin.Forms e é preferido por muitos programadores que trabalham com Xamarin.Forms. No entanto, a abordagem SAP também é suportada e definitivamente tem seus defensores também.

Mas por que escolher? Pode ter ambos na mesma solução. Se você criou uma solução Xamarin.Forms solução com um projeto recurso compartilhado, você pode adicionar um novo projeto PCL para a solução, selecionando a biblioteca de classes (Xamarin.Forms Portable) modelo. Os projetos de aplicativos podem acessar tanto a SAP e PCL, ea SAP pode acessar o PCL também.