## A estrutura Color {#a-estrutura-color}

Internamente, a estrutura Color armazena cores em duas maneiras diferentes:

*   Como valores de vermelho, verde e azul (RGB) do tipo double que variam de 0 a 1\. Propriedades somente leitura chamadas R, G e B exibem estes valores.
*   Como valores de matiz, saturação e luminosidade do tipo double, que também variam de 0 a 1\. Estes valores são exibidos com propriedades somente leitura chamadas Hue, Saturation e Luminosity.

A estrutura Color também suporta um canal alfa para indicar graus de opacidade. Uma propriedade somente leitura chamada de A exibe este valor, que varia de 0 para transparente até 1 para opaco. Todas estas propriedades são somente leitura. Uma vez criada, um valor de Color é imutável.

Você pode criar um valor de Color em uma das várias maneiras. Os três construtores são as mais fáceis:

*   **new Color(double grayShade)**
*   **new Color(double r, double g, double b)**
*   **new Color(double r, double g, double b, double a)**

Os argumentos podem variar de 0 a 1\. Color também define vários métodos de criação estáticos, incluindo:

*   **Color.FromRgb(double r, double g, double b)**
*   **Color.FromRgb(int r, int g, int b)**
*   **Color.FromRgba(double r, double g, double b, double a)**
*   **Color.FromRgba(int r, int g, int b, int a)**
*   **Color.FromHsla(double h, double s, double l, double a)**

Os dois métodos estáticos com argumentos inteiros aceitam valores entre 0 e 255, que é normalmente a representação das cores RGB. Internamente, o construtor simplesmente divide valores inteiro por 255.0 para converter para double.

Cuidado! Você pode pensar que está criando uma cor vermelha com esta chamada:

**Color.FromRgb(1, 0, 0)**

No entanto, o compilador C# assumirá que estes valores são inteiros. O método de inteiro será invocado, e o primeiro argumento será dividido por 255.0, com um resultado perto de zero. Se você quer usar o método que tem argumentos double, seja explícito:

**Color.FromRgb(1.0, 0, 0)**

Color também define métodos de criação estáticos para um pacote no formato uint e um no formato hexadecimal em uma string, mas estes são menos usados.

A estrutura Color também define 17 campos somente leitura estáticos e públicos do tipo Color. Na tabela a seguir, os valores RGB inteiro que a estrutura Color utiliza internamente para definir estes campos são mostrados juntos com os valores correspondentes de Hue, Saturation, e Luminosity, com arredondamento para maior clareza.

Com exceção do Pink, você pode reconhecer estas cores como os nomes de cor suportadas em HTML. Um 18º campo somente leitura estático e público é chamado de Transparent, o qual tem todas as propriedades R, G, B definidas com zero.

Quando as pessoas têm a oportunidade de formular interativamente uma cor, o modelo de cor HSL é muitas vezes mais intuitivo que RGB. Os ciclos Hue atravessam as cores do espectro visível (e do arco-íris) começando com vermelho em 0, verde em 0.33, azul em 0.67, e volta ao vermelho em 1.

A Saturation indica o grau da matiz na cor, que varia de 0, que é sem matiz e resulta em um tom de cinza, até 1 para total saturação.

A Luminosity é uma medida de luminosidade, variando de 0 para preto e 1 para branco.

Programas de seleção de cores no Capítulo 15, “Interface interativa”, permite-lhe explorar os modelos RGB e HSL mais interativamente.

A estrutura Color contem vários exemplos de métodos interessantes que permitem criar novas cores que são modificações de cores existentes:

*   **AddLuminosity(double delta)**
*   **MultiplyAlpha(double alpha)**
*   **WithHue(double newHue)**
*   **WithLuminosity(double newLuminosity)**
*   **WithSaturation(double newSaturation)**

Finalmente, Color define duas propriedades especiais somente leitura estáticas do tipo Color:

*   **Color.Default**
*   **Color.Accent**

A propriedade Color.Default é usada extensivamente com Xamarin.Forms para definir a cor padrão das visões. A classe VisualElement inicializa sua propriedade BackgroundColor com Color.Default, e a classe Label inicializa sua propriedade TextColor com Color.Default.

No entanto, Color.Default é um valor de Color com todas suas propriedades R, G, B e A definidas com -1, o que significa ser um valor “falso” especial que não significa nada em sí, mas indica que o valor real é específico da plataforma.

Para Label e ContentPage (e a maioria das classes que derivam de VisualElement), a propriedade BackgroundColor definida com Color.Default significa transparente.

No Label, o valor de TextColor com Color.Default significa preto em um dispositivo iOS, branco em um dispositivo Android, e branco ou preto em Windows Phone, dependendo da cor do tema selecionado pelo usuário.

Sem você escrever código que explore os objetos de interface do usuário específicos da plataforma, seu programa Xamarin.Forms não pode determinar se o esquema de cores básico é branco sobre preto ou preto sobre branco. Isto pode ser um pouco frustrante se você quiser criar cores que são compatíveis com o esquema de cores ─ por exemplo, uma cor de texto azul escuro se o fundo padrão for branco, ou um texto na cor amarelo claro se o fundo for escuro.

Você tem duas estratégias para trabalhar com cor: Você pode escolher por fazer sua programação Xamarin.Forms de uma maneira bem independente da plataforma e evitar fazer qualquer suposição sobre o esquema de cores padrão de qualquer telefone. Ou, você pode usar o seu conhecimento sobre os esquemas de cores das várias plataformas e usar Device.OnPlataform para especificar as cores específicas da plataforma.

Mas não tente simplesmente ignorar todos os padrões da plataforma e defina explicitamente todas as cores em seu aplicativo para seu próprio esquema de cores. Isto provavelmente não funcionará bem como você espera porque muitas visões usam outras cores que se relacionam com o tema de cor do sistema operacional que não são expostos através das propriedades Xamarin.Forms.

Uma opção simples é usar a propriedade Color.Accent como uma cor de texto alternativa. Nas plataformas iPhone e Android, é uma cor que é visível sobre o fundo padrão, mas não é a cor de texto padrão. No Windows Phone, é uma cor selecionada pelo usuário como parte do tema de cor.

Você pode ter texto semitransparente ao definir TextColor para um valor de Color com uma propriedade A menor que 1\. No entanto, se você quer uma versão semitransparente da cor de texto padrão, use a propriedade Opacity do Label como alternativa. Esta propriedade é definida pela classe VisualElement e tem 1 como valor padrão. Defina-o com valores menores que 1 para ter vários graus de transparência.