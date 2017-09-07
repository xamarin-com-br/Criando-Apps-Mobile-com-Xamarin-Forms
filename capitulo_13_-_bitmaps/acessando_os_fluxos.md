## Acessando os fluxos {#acessando-os-fluxos}

O programa Streams Bitmap contém um arquivo XAML com dois elementos de imagem à espera de bitmaps, cada um dos quais é definido no arquivo code-behind usando ImageSource.FromStream:

A primeira imagem é definida a partir de um recurso incorporado no PCL; o segundo é definido a partir de um bitmap acessado pela web.

No programa BlackCat no Capítulo 4, &quot;Rolagem a pilha,&quot; você viu como obter um objeto Stream para qualquer recurso armazenado com um Build Action do EmbeddedResource no PCL. Você pode usar a mesma técnica para acessar um bitmap armazenado como um recurso incorporado:

O argumento para ImageSource.FromStream é definido como uma função que retorna um objeto Stream, de modo que o argumento é aqui expressado como uma função lambda. A chamada para o método GetType retorna o tipo da classe BitmapStreamsPage e GetTypeInfo fornece mais informações sobre esse tipo, incluindo o objeto Assembly que contém o tipo. Essa é a montagem BitmapStream PCL, que é o conjunto com o recurso incorporado. GetManifestResourceStream retorna um objeto Stream, que é o valor de retorno que ImageSource.FromStream quer.

Se você precisar de um pouco de ajuda com os nomes desses recursos, o GetManifestResourceNames retorna uma matriz de objetos de cadeia com todos os IDs de recursos no PCL. Se você não consegue descobrir por que seu GetManifestResourceStream não está funcionando, primeiro certifique-se de seus recursos têm um Build Action do EmbeddedResource, e depois chamar GetManifestResourceNames para obter todos os IDs de recursos.

Para fazer download de um mapa de bits através da web, você pode usar o mesmo método WebRequest demonstrado anteriormente no programa ImageBrowser. Neste programa, o retorno BeginGetResponse é uma função lambda:

O retorno de chamada BeginGetResponse também contém mais duas funções lambda embutidos! A primeira linha do retorno de chamada obtém o objeto de fluxo para o bitmap. Este objeto Stream não é bastante adequado para Windows Runtime para que os conteúdos são copiados para um MemoryStream.

A próxima instrução utiliza uma função lambda curta como o argumento para ImageSource.FromStream para definir uma função que retorna esse fluxo. A última linha do retorno de chamada BeginGetResponse é uma chamada para Device.BeginInvokeOnMainThread para definir o objeto ImageSource para a propriedade origem da imagem.

Pode parecer como se você tem mais controle sobre o download de imagens usando WebRequest e ImageSource.FromStream do que com ImageSource.FromUri, mas o método ImageSource.FromUri tem uma grande vantagem: ele armazena os bitmaps baixados em uma área de armazenamento privado para a aplicação. Como você viu, você pode desativar o cache, mas se você estiver usando Image- Source.FromStream vez de ImageSource.FromUri, você pode encontrar a necessidade de armazenar em cache Images e que seria um trabalho muito maior.