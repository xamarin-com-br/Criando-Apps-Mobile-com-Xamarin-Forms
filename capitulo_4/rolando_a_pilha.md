## Rolando a pilha {#rolando-a-pilha}

Se você é como a maioria dos programadores, assim que viu a lista estática das propriedades _Color_ no capítulo anterior, quis escrever um programa para exibir todos eles, talvez usando a propriedade _Text_ da _Label_ para identificar a cor, e a propriedade TextColor para mostrar a cor real.

Embora você possa fazer isso com uma única _Label_ utilizando um objeto _FormattedString_, é muito mais fácil com vários objetos _Label_. Porque vários objetos de _Label_ estão envolvidos, este trabalho também requer alguma forma de exibir todos os objetos _Label_ na tela.

A classe _ContentPage_ define uma propriedade de conteúdo do tipo de visualização que você pode definir para um objeto - mas apenas um objeto. Exibir múltiplas _views_ exige a criação de conteúdo para uma instância de uma classe que pode ter vários herdeiros de tipo _View_. Essa classe é a Layout&lt;T&gt;, que define uma propriedade _Children_ do tipo IList &lt;T&gt;.

A classe Layout &lt;T&gt; é abstrata, mas quatro classes derivam de Layout &lt;View&gt;, uma classe que pode ter várias crianças do tipo _View_. Em ordem alfabética, estas quatro classes são:

*   **AbsoluteLayout**
*   **Grid**
*   **RelativeLayout**
*   **StackLayout**

Cada uma delas organiza seus filhos de uma forma característica. Este capítulo centra-se na StackLayout