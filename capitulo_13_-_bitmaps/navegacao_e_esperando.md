## Navegação e esperando {#navega-o-e-esperando}

Outra característica da imagem é demonstrada no programa ImageBrowser, que permite que você navegue as fotos utilizados para alguns dos exemplos neste livro. Como você pode ver no seguinte arquivo XAML, um elemento de imagem divide a tela com um rótulo e duas visões do botão. Observe que um manipulador Changed propriedade- está definido na Imagem. Você aprendeu no Capítulo 11, &quot;A infra-estrutura bindable&quot;, que o manipulador de PropertyChanged é implementado por bindableobject e é acionado sempre que uma propriedade bindable altera o valor.

Também nesta página é um ActivityIndicator. Eles geralmente usam este elemento quando um programa está à espera de uma longa operação seja concluída (como o download de um mapa de bits), mas não pode fornecer qualquer informação sobre o progresso da operação. Se o seu programa sabe o que fracção da operação for concluída, você pode usar um ProgressBar em vez disso. (ProgressBar é demonstrado no próximo capítulo).

O ActivityIndicator tem uma propriedade booleana denominada IsRunning. Normalmente, essa propriedade é falsa eo ActivityIndicator é invisível. Defina a propriedade como true para fazer as ActivityIn- dicator visível. Todas as três plataformas implementar uma animação visual para indicar que o programa está funcionando, mas parece um pouco diferente em cada plataforma. No iOS é uma roda de fiar, e sobre Android é um círculo parcial spinning. Em dispositivos Windows, uma série de pontos move pela tela.

Para fornecer navegando acesso às imagens, o ImageBrowser precisa baixar um arquivo JSON com uma lista de todos os nomes de arquivos. Ao longo dos anos, várias versões do .NET introduziram várias classes capazes de baixar objetos através da web. No entanto, nem todos estes estão disponíveis na versão do .NET que está disponível em uma biblioteca de classes portátil que tem o perfil compatível com Xamarin.Forms. Uma classe que está disponível é WebRequest e sua classe descendente HttpWebRequest.

O método WebRequest.Create retorna um método WebRequest com base em um URI. (O valor de retorno é realmente um objeto HttpWebRequest.) O método BeginGetResponse requer uma função de retorno de chamada que é chamada quando o fluxo de referência a URI está disponível para acesso. O fluxo é ac- cessible de uma chamada para EndGetResponse e GetResponseStream.

Uma vez que o programa obtém acesso ao objeto de fluxo no código a seguir, ele usa a classe DataCon- tractJsonSerializer juntamente com a classe ImageList incorporado definida perto do topo da classe ImageBrowserPage para converter o arquivo JSON para um objeto ImageList:

O corpo inteiro do método WebRequestCallback é fechado em uma função lambda que é o argumento para o método Device.BeginInvokeOnMainThread. WebRequest baixa o arquivo Referenced pela URI em um thread secundário de execução. Isso garante que a operação não bloqueia thread principal do programa, que está a lidar com a interface do usuário. O método de retorno também executa neste segmento secundário. No entanto, objetos de interface do usuário em um aplicativo Xamarin.Forms só pode ser acessado a partir do thread principal.

O objetivo do método Device.BeginInvokeOnMainThread é para contornar este problema. O argumento para este método está na fila para ser executado no thread principal do programa e pode acessar com segurança os objetos de interface do usuário.

Quando você clica nos dois botões, as chamadas para FetchPhoto usar UriImageSource o download de um novo mapa de bits. Isso pode demorar um segundo ou assim. A classe de imagem define uma propriedade booleana denominada IsLoading isso é verdade quando a imagem está em processo de carga (ou download) um bitmap. IsLoading é apoiado pelo IsLoadingProperty propriedade bindable. Isso também significa que sempre que IsLoading valor mudanças, um evento PropertyChanged é acionado. O programa usa o evento PropertyChanged handler- o método OnImagePropertyChanged na parte inferior da classe para definir a propriedade IsRunning do ActivityIndicator para o mesmo valor que a propriedade IsLoading de Imagem.

Você verá no Capítulo 16, &quot;Ligação de dados&quot;, como seus aplicativos podem vincular propriedades como IsLoading e IsRunning para que eles mantêm o mesmo valor, sem quaisquer manipuladores de eventos explícitos.

Aqui está ImageBrowser em ação:

Algumas das imagens tem um EXIF bandeira conjunto orientação, e se a plataforma em particular ignora essa bandeira, a imagem é exibida para os lados.

Se você executar este programa em modo paisagem, você vai descobrir que os botões desaparecem. A melhor layout opção para este programa é uma grade, o que é demonstrado no capítulo 17.