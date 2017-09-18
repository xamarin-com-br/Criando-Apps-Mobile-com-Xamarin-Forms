## Herança de estilo {#heran-a-de-estilo}

O "**TargetType**" do estilo faz duas funções diferentes: Uma dessas funções é descrita em a próxima seção sobre estilos implícitos. A outra função é para o benefício do analisador "**XAML**". O analisador "**XAML**" deve ser capaz de resolver os nomes de propriedades nos objetos "**Setter**" e para isso ele precisa que seja fornecido o nome de uma classe pelo "**TargetType**".

Todas as propriedades no estilo devem ser definidas por ou herdadas pela classe especificada na propriedade "**TargetType**". O tipo do elemento visual no qual o estilo é definido deve ser o mesmo "**TargetType**" ou uma classe derivada de "**TargetType**".

Se você precisar de um estilo apenas para propriedades definidas por "**View**", você pode definir o "**TargetType**" para "**View**" e ainda usar o estilo nos botões ou qualquer outro derivado de "**View**", como nesta versão modificada do programa "**BasicStyle**":

![](/assets/12-17-HerancaEstilo.png)

Como você pode ver, o mesmo estilo é aplicado a todos os "**Button**" e "**Label**" filhos do "**StackLayout**" :

![](/assets/12-18--TelasHerancaEstilo.png)

Como você pode ver, o mesmo estilo é aplicado a todos os filhos **Button **e **Label **do **StackLayout**:

Mas supomos que agora você quer expandir este estilo, mas de forma diferente para "**Button**" e "**Label**". Isso é possível?

Sim. Os estilos podem derivar de outros estilos. A classe estilo inclui uma propriedade chamada "**BasedOn**" do tipo estilo. No código, você pode definir essa propriedade "**BasedOn**" diretamente a outro objeto estilo. Em "**XAML**" você definir o atributo "**BasedOn**" para uma extensão de marcação "**StaticResource**" que faz referência a um estilo criado anteriormente. O novo estilo pode incluir objetos "**Setter**" para novas propriedades ou usá-las para substituir propriedades no estilo anterior. O estilo "**BasedOn**" deve ter como alvo a mesma classe ou uma classe ancestral do novo estilo "**TargetType**".

Aqui está o arquivo XAML para um projeto chamado "**StyleInheritance**". O aplicativo faz uma referência para"**Xamarin.FormsBook.Toolkit"** para duas finalidades: ele usa a extensão de marcação "**HslColor**" para demonstrar que as extensões de marcação são ajustes dos valores legítimos em objetos "**Setter**" e para demonstrar que um estilo pode ser definido para uma classe customizada, neste caso "**AltLabel**".

O "**ResourceDictionary**" contém quatro estilos : O primeiro tem uma chave de dicionário de "**VisualStyle**". O estilo com a chave do dicionário de "**baseStyle**" deriva de " **VisualStyle**". Os estilos com teclas de "**labelStyle **"e" **ButtonStyle**" derivam de "**baseStyle** ".

![](/assets/12-19-ExemploEstilot.png)![](/assets/12-19-ExemploEstilo1.png)![](/assets/12-19-ExemploEstilo2.png)

O Estilo do Botão demonstra como definir uma propriedade de valor de um "**Setter**" a um objeto "**OnPlatform**":

Imediatamente após a seção de recursos uma marcação define a propriedade estilo da página com o estilo "**VisualStyle**":



Como a página deriva de "VisualElement" mas de "View", este é o único estilo no recurso dicionário que pode ser aplicada para a página. No entanto, o modelo não pode ser aplicado a página até depois da seção de recursos, portanto, usar o elemento "form" de "StaticResource" é uma boa solução aqui. O fundo inteiro da página é colorido com base neste estilo, e o estilo também é herdado por todos os outros estilos:

Se o estilo para o "AltLabel" só incluiu objetos "setter" para propriedades definidas por "Label", o "Targettype" poderia ser "Label" em vez de "AltLabel". Mas o estilo tem um "Setter" para a propriedade "PointSize". Essa propriedade é definida por "AltLabel" de modo que o "TargetType" deve ser "tookit:AltLabel".

Um "setter" pode ser definido para a propriedade "PointSize" porque "PointSize" é apoiado por uma propriedade "bindable". Se você muda a acessibilidade do objeto "BindableProperty" em "AltLabel" de público para privado, a propriedade continuará a funcionar para muitos usos rotineiros de "AltLabel", mas agora "PointSize" pode não ser um "Setter" de estilo. O analisador XAML se queixa de que ele não pode encontrar "PointSizeProperty", que é a propriedade "bindable" que faz a propriedade "PointSize".

Você descobriu no capítulo 10 como "StaticResource" funciona: Quando o analisador "XAML" encontra uma extensão de marcação "StaticResource" ele busca uma árvore visual para uma chave de dicionário. Este processo tem implicações para estilos. Você pode definir um estilo em uma seção de recursos e em seguida, sobreescrever capítulo 12 estilos 252 com outro estilo com a mesma chave de dicionário em uma seção de recursos diferente localizada mais abaixo na árvore visual. Quando você definir a propriedade "BasedOn" para uma extensão de marcação "StaticResource", o estilo que você está derivando deve ser definido na mesma seção Recursos \(como demonstrado no programa "StyleInheritance" \) ou uma seção de recursos superior na árvore visual.

Isto significa que você pode estruturar seus estilos em "XAML" de duas maneiras hierárquicas: você pode usar "BasedOn" para derivar estilos de outros estilos, e você pode definir estilos em diferentes níveis na árvore visual que derivam de estilos mais elevados na árvore visual ou substituí-los por completo.

Para aplicações maiores, com várias páginas e muita marcação, a recomendação para a definição de estilos é muito simples: definir seus estilos tão perto quanto possível dos elementos que usam esses estilos.

Aderir a estas recomendações para manter o programa é particularmente importante quando se trabalha com estilos implícitos.

