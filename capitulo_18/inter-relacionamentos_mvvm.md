## Inter-relacionamentos MVVM {#inter-relacionamentos-mvvm}

MVVM divide um aplicativo em três camadas:

O modelo fornece dados subjacentes, às vezes envolvendo arquivo ou web acessos.

O ViewModel liga o Modelo a. Ele ajuda a gerenciar os dados do modelo para torná-lo mais receptivo à Vista, e vice-versa.

A vista é a camada de interface do usuário ou apresentação, geralmente implementado em XAML.

O modelo não conhece o ViewModel. Em outras palavras, o modelo não sabe nada sobre as propriedades publicas e métodos do ViewModel, e certamente nada sobre seu funcionamento interno. Da mesma forma, o ViewModel não conhece a View. Se toda a comunicação entre as três camadas ocorre por meio de chamadas de método e propriedade acessos, em seguida, chama em uma única direção são permitidos. O View só faz chamadas para o ViewModel ou acessa propriedades do ViewModel eo ViewModel semelhante só faz chamadas para o modelo ou acessa Propriedades modelo:

Estas chamadas de método permitem que a View para obter informações do ViewModel, que por sua vez recebe informações do modelo.

Em ambientes modernos, no entanto, os dados são muitas vezes dinâmicos. Muitas vezes, o modelo vai obter mais ou mais recente dados que devem ser comunicados ao ViewModel e eventualmente para a View. Por esta razão, o Vista pode anexar manipuladores de eventos que estão sendo implementados no ViewModel eo ViewModel pode anexar manipuladores de eventos definidos pelo modelo. Isto permite uma comunicação bidireccional, escondendo a View do ViewModel e o ViewModel do Modelo:

MVVM foi projetado para tirar vantagem de XAML e ligações de dados particularmente baseados em XAML. Geralmente, a vista “page class” que usa XAML para construir a interface do usuário. Portanto, a conexão entre a View e o ViewModel consiste em grande parte, e talvez exclusivamente, de ligações de dados baseados em XAML:

Os programadores que estão muito apaixonados por MVVM muitas vezes têm uma meta informal de expressar todas as interações entre o Vista eo ViewModel em uma classe página com ligações de dados baseada em XAML, e no processo de reduzir o código da página de arquivo para um simples código subjacente chamada InitializeComponent. Este objetivo é difícil de alcançar na programação da vida real, mas é um prazer quando isso acontece.

Pequenos programas, tais como aqueles em um livro como este, muitas vezes tornam-se maiores quando MVVM é introduzido. Não deixe que isso desencorajar o seu uso do MVVM! Utilize os exemplos aqui para ajudá-lo a determinar como MVVM pode ser usado em um programa maior, e se você verá que isto ajuda enormemente na arquitetura de suas aplicações.