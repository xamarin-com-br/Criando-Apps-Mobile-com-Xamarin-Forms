## Estilos de dispositivo {#estilos-de-dispositivo}

&quot;Xamarin.Forms&quot; inclui seis estilos dinâmicos internos. Estes são conhecidos como estilos de dispositivos, e eles são membros de uma classe aninhada de dispositivos chamado &quot;Styles&quot;. Esta classe &quot;Styles&quot; define 12 campos estáticos e apenas leitura que ajudam a referenciar essas seis estilos em código :

*   **&quot;BodyStyle&quot; do tipo &quot;Style&quot;.**
*   **&quot;BodyStyleKey&quot; do tipo &quot;string&quot; e igual a “BodyStyle”.**
*   **&quot;TitleStyle&quot; do tipo &quot;Style&quot;.**
*   **&quot;TitleStyleKey&quot; do tipo &quot;string&quot; e igual a “TitleStyle”.**
*   **&quot;SubtitleStyle&quot; do tipo &quot;Style&quot;.**
*   **&quot;SubtitleStyleKey&quot; do tipo &quot;string&quot; e igual a “SubtitleStyle”.**
*   **&quot;CaptionStyle&quot; do tipo &quot;Style&quot;.**
*   **&quot;CaptionStyleKey&quot; do tipo &quot;string&quot; e igual a “CaptionStyle”.**
*   **&quot;ListItemTextStyle&quot; do tipo &quot;Style&quot;.**
*   **&quot;ListItemTextStyleKey&quot; do tipo &quot;string&quot; e igual a “ListItemTextStyle”.**
*   **&quot;ListItemDetailTextStyle&quot; do tipo &quot;Style&quot;.**
*   **&quot;ListItemDetailTextStyleKey&quot; do tipo &quot;string&quot; e igual a “ListItemDetailTextStyle”.**

Todos os seis estilos têm um &quot;TargetType&quot; do tipo &quot;Label&quot; armazenados em um dicionário, mas não um dicionário aonde aplicações podem acessar diretamente.

No código, você deve usar os campos nesta lista para acessar os estilos de dispositivo. Por exemplo, você pode definir o objeto &quot;Device.Styles.BodyStyle&quot; diretamente na propriedade estilo de um &quot;Label&quot; para o texto que pode ser apropriado para o corpo de um número. Se você estiver definindo um estilo no código que deriva de um desses estilos de dispositivo, defina o &quot;BaseResourceKey&quot; para &quot;Device.Styles.BodyStyleKey&quot; ou simplesmente &quot;BodyStyle&quot; se você não está com medo de errar.

Em &quot;XAML&quot;, você irá simplesmente usar a tecla texto &quot;BodyStyle&quot; com &quot;DynamicResource&quot; para configurar este estilo para a propriedade estilo de um &quot;Label&quot; ou para definir &quot;BaseResourceKey&quot; ao configurar o estilo de &quot;Device.Styles.BodyStyle&quot;.

O programa &quot;DeviceStylesList&quot; demonstra como acessar esses estilos e como definir um novo estilo que herda de &quot;SubtitleStyle&quot; tanto em &quot;XAML&quot; como no código. Aqui está o arquivo XAML:

O &quot;StackLayout&quot; contém duas combinações de &quot;Label&quot; e &quot;BoxView&quot; ( uma na parte superior e um na inferior) para exibir cabeçalhos sublinhados. Após o primeiro desses cabeçalhos, elementos &quot;Label&quot; referenciam os estilos de dispositivo com &quot;DynamicResource&quot;. O novo estilo de legenda é definido no dicionário de recursos para a página.

O arquivo &quot;code-behind&quot; acessa os estilos de dispositivo usando as propriedades na classe &quot;Device.Styles&quot; e cria um novo estilo derivando de &quot;SubtitleStyle&quot;:

O código e &quot;XAML&quot; resultam em estilos idênticos, é claro, mas cada plataforma implementa estes estilos de dispositivo de uma forma diferente:

No IOS , o código subjacente implementa estes estilos principalmente com propriedades estáticas da classe &quot;UIFont&quot;. No Android, a implementação envolve principalmente propriedades &quot;TextAppearance&quot; da classe &quot;Resource.Attribute&quot;. No Windows Phone, são recursos de aplicativos com chaves começando com &quot;PhoneText&quot;.

A natureza dinâmica destes estilos é mais facilmente demonstrada no iOS : enquanto o programa &quot;DeviceStyles&quot; está em execução, toque no botão &quot;Home&quot; e execute a chamada das configurações (&quot;Settings&quot;). Escolha o item &quot;General&quot;, em seguida, &quot;Acessibility&quot;, e &quot;Larger Text&quot;. Um controle deslizante está disponível para fazer o texto menor ou maior. Mude esse controle, toque duas vezes no botão &quot;Home&quot; para mostrar as aplicações atuais e selecione &quot;DeviceStyles&quot; novamente. Você vai ver o conjunto do texto a partir de estilos dispositivo (ou os estilos que derivam de estilos de dispositivo) mudar o tamanho, mas nenhum dos textos sem estilo alteram de tamanho. Novos objetos substituíram os estilos de dispositivo no dicionário.

A natureza dinâmica dos estilos de dispositivo não é tão óbvia no Android porque as alterações ao item de tamanho de fonte da seção de exibição em configurações afetam todos os tamanhos de fonte em um programa &quot;Xamarin.Forms&quot;. No Windows Phone, o item tamanho de texto na sessão facilidade de acesso da sessão configurações só tem função nas aplicações que usam a API do &quot;Windows Runtime&quot;. &quot;Xamarin.Forms&quot; atualmente usa a API do &quot;Silverlight&quot;.

O próximo capítulo inclui um programa que demonstra como fazer um pequeno leitor de livros eletrônicos que permite você ler um capítulo de Alice no País das Maravilhas. Este programa usa estilos de dispositivo para controlar a formatação de todo o texto, incluindo o livro e títulos de capítulos.

Mas o que este pequeno leitor de livros eletrônicos inclui também são ilustrações, e isso exige uma exploração em objetos de &quot;bitmaps&quot;.