## Construtor com argumentos {#construtor-com-argumentos}

Para passar argumentos para um construtor de um elemento em XAML, o elemento deve ser separado em _tags_ de início e fim. Inicialize as _tag_ de início e fim do elemento com x:Arguments. Dentro das _tags_ x:Arguments inclua um ou mais argumentos do construtor.

Mas como especificar vários argumentos dos tipos comuns, como _double_ ou _int_? Você separaria os argumentos com vírgulas?

Não. Cada argumento deve ser delimitado com _tags_ de início e fim. Felizmente, a especificação XAML 2009 define elementos XML para tipos básicos comuns. Você pode usar essas _tags_ para esclarecer os tipos de elementos, para especificar os tipos genéricos em OnPlatform, ou para delimitar argumentos do construtor. Confira o conjunto completo suportado por Xamarin.Forms. Observe que foram duplicados os nomes de tipos .NET ao invés dos nomes de tipos C#:

*   **x:Object**
*   **x:Boolean**
*   **x:Byte**
*   **x:Int16**
*   **x:Int32**
*   **x:Int64**
*   **x:Single**
*   **x:Double**
*   **x:Decimal**
*   **x:Char**
*   **x:String**
*   **x:TimeSpan**
*   **x:Array**
*   **x:DateTime (Suportado pelo Xamarin.Forms mas não por especificação do XAML 2009)**

Você será pressionado para encontrar uso para todos eles, mas provavelmente vai descobrir usos para apenas alguns deles.

O **ParameteredConstructorDemo** exemplo, a seguir, demonstra o uso de x:Arguments com argumentos delimitados por _tags_ x:Double usando 3 diferentes construtores da estrutura Color. O construtor requer 3 parâmetros vermelho, verde e azul com os valores que variam de 0 a 1\. Construtor com 4 parâmetros (que é definido aqui como 0,5), e o construtor com parâmetro único indica uma sombra de 0 (preto) a 1 (branco):

O número de elementos dentro de _tags_ x:Arguments, e os tipos destes elementos, deve corresponder a um dos construtores da classe ou estrutura. Aqui está o resultado:

O BoxView azul é mais claro no fundo branco e escuro no fundo preto porque a transparência dele é de 50% deixando transparecer o fundo da tela.