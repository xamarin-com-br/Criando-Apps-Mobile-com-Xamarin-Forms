## PCL ou SAP? {#pcl-ou-sap}

Quando criou a solução **Hello**, você tinha uma escolha de dois modelos de aplicação:

*   **Blank App (Xamarin.Forms Portable)**
*   **Blank App (Xamarin.Forms Shared)**

A primeira cria uma Biblioteca de Classes Portátil (PCL), enquanto o segundo cria um projeto recurso compartilhado (SAP), consistindo apenas em arquivos de código compartilhado. A solução original **Hello** utiliza o modelo de PCL. Agora vamos criar uma segunda solução nomeado **HelloSap** com o modelo de SAP.

Como você vai ver, tudo parece praticamente o mesmo, exceto que o próprio projeto **HelloSap** contém apenas um item: o arquivo App.cs.

Tanto as abordagens PCL e SAP, o código é compartilhado entre as três aplicações, mas de maneiras diferentes: Com a aproximação PCL, todo o código comum é empacotado em uma bilbioteda de dynamic-link que cada projeto referência na aplicação e liga em tempo de execução. Com a abordagem SAP, os arquivos de código comuns são efetivamente incluídos com os três projetos de aplicação em tempo de compilação. Por padrão, a SAP tem apenas um único arquivo chamado App.cs, mas de forma eficaz, é como se não existisse esse projeto **HelloSap** e, em vez havia três cópias diferentes deste arquivo nos três projetos da aplicação.

Alguns problemas sutis (e não tão sutis) podem manifestar-se com a **Blank App (Xamarin.Forms Shared)** template:

Os projetos iOS e Android têm acesso a praticamente a mesma versão do .NET, mas não é a mesma versão do .NET que um projeto do Windows Phone usa. Isto significa que quaisquer classes .NET acessado pelo código partilhado pode ser um pouco diferente, dependendo da plataforma. Como você vai descobrir mais tarde neste livro, este é o caso de algum arquivo I / O classes e o namespace System.IO.

Você pode compensar estas diferenças, usando diretivas de pré-processador C#, particularmente #if e #elif. Nos projetos gerados pelo modelo Xamarin.Forms, os projetos Windows Phone e iPhone definem símbolos que você pode usar com estas diretivas.