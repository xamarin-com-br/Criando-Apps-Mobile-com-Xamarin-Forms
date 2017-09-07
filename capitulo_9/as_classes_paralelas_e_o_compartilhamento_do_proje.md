## As classes paralelas e o compartilhamento do projeto {#as-classes-paralelas-e-o-compartilhamento-do-projeto}

Embora o compartilhamento do projeto seja uma extensão da plataforma, a relação vai nos dois sentidos: da mesma maneira que um projeto da plataforma pode fazer chamadas em código em um projeto de recurso compartilhado, o compartilhamento do projeto pode fazer chamadas para as plataformas individuais.

Isto significa que pode restringir as chamadas de API específicas da plataforma para classes nos projetos das plataformas individuais. Se os nomes e namespaces dessas classes nos projetos da plataforma são os mesmos, então o código no compartilhamento do projeto pode acessar essas classes de uma maneira independente e transparente.

Na solução DisplayPlatformInfoSap2, cada um dos três projetos de plataformas tem uma classe chamada PlatformInfo que contém dois métodos que retornam strings nomeadas getModel e GetVersion. Está aqui a versão desta classe no projeto iOS:

Observe o nome do namespace. Embora as outras classes neste projeto iOS usar o namespace DisplayPlatformInfoSap2.iOS, o namespace para esta classe é apenas DisplayPlatformInfoSap2\. Isso permite que a compartilhamento do projeto consiga acessar essa classe diretamente, sem qualquer plataforma específica.

Aqui está à classe paralela no projeto Android. Mesmos nomes de métodos mesmo namespace, mesmo nome da classe, mas diferentes implementações destes métodos usando chamadas de API do Android:

E aqui está o Windows Phone:

O arquivo XAML no projeto DisplayPlatformInfoSap2 é basicamente o mesmo que o de projeto DisplayPlatformInfoSap1\. O arquivo code-behind é consideravelmente mais simples:

A versão específica do PlatformInfo que é referenciado pela classe é única no projeto compilado. É quase como se nós tivéssemos definido uma pequena extensão para Xamarin.Forms que reside nos projetos de plataformas individuais.