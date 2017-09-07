## DynamicResource para fins especiais {#dynamicresource-para-fins-especiais}

Uma alternativa para StaticResource para fazer referência a itens do dicionário de Resources é DynamicResource e se você apenas substituir DynamicResource por StaticResource no exemplo mostrado acima, o programa irá executar aparentemente da mesma forma.

No entanto, as duas extensões de marcação são muito diferentes. StaticResource acessa o item no dicionário apenas uma vez, enquanto o XAML está sendo analisado e a página está sendo construída. Mas DynamicResource mantém uma ligação entre o dicionário chave e a propriedade definida a partir desse item de dicionário. E se o item no dicionário de recurso referenciado pelas chave mude, DynamicResource que irá detectar a mudança e definir o novo valor à propriedade.

Cético? Vamos testá-lo. O projeto DynamicVsStatic tem um arquivo XAML que define um item de recurso do tipo string com uma chave de CURRENTDATETIME, mesmo que o item no dicionário seja a string &quot;Não é realmente um DateTime&quot;!

Este item do dicionário é referenciado quatro vezes no arquivo XAML, mas uma das referências é descomentada. Nos dois primeiros exemplos, a propriedade Text de uma Label é definida utilizando StaticResource e DynamicResource . No segundo dois exemplos, a propriedade Text de um objeto Span é definida de forma semelhante, mas o uso de DynamicResource no Span aparece nos comentários:

Você provavelmente vai esperar todas as três referências para o item de dicionário CURRENTDATETIME para resultar na exibição do texto &quot;Não realmente um DateTime&quot;. No entanto, o arquivo de code-behind inicia um temporizador. A cada segundo, o retorno de chamada timer substitui o item de dicionário com uma nova seqüência que representa um valor real de DateTime:

O resultado é que as propriedades Text definidas com StaticResource permanecem as mesmas, enquanto o outro com DynamicResource muda a cada segundo para refletir o novo item no dicionário:

Aqui está outra diferença: se não houver nenhum item no dicionário com o nome de chave especificado, StaticResource irá lançar uma exceção em tempo de execução, mas DynamicResource não irá.

Você pode tentar descomentar o bloco de marcação no final do projeto DynamicVsStatic, e você realmente vai encontrar uma exceção de tempo de execução no sentido de que a propriedade Text não poderia ser encontrada. Apenas improvisadamente, esta exceção não soa muito bem, mas ele está se referindo a uma diferença muito real.

O problema é que as propriedades Text na Label e Span são definidas de diferentes formas, e que essa diferença importa muito para DynamicResource . Essa diferença será explorada no próximo capítulo, &quot;A infraestrutura bindable.&quot;