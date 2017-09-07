## Geração de bitmaps em tempo de execução {#gera-o-de-bitmaps-em-tempo-de-execu-o}

Todas as três plataformas suportam o formato de arquivo BMP, que remonta ao início do Microsoft Windows. Apesar de sua herança antiga, o formato de arquivo BMP é agora bastante padronizada com informações de cabeçalho mais extenso.

Embora existam algumas opções BMP que permitem alguma compressão rudimentar, a maioria dos arquivos BMP é descompactado. Esta falta de compressão é geralmente considerada como uma desvantagem da BMP para- esteira, mas em alguns casos, não é uma desvantagem em todos. Por exemplo, se você deseja gerar um bitmap algoritmo em tempo de execução, é muito mais fácil para gerar um mapa de bits não comprimido em vez de um dos formatos de arquivos compactados. (Na verdade, mesmo se você tivesse uma função de biblioteca para criar um arquivo JPEG ou PNG, você aplicar essa função para os dados de pixel não-comprimido.)

Você pode criar um bitmap de algoritmos em tempo de execução, preenchendo um MemoryStream com os cabeçalhos de arquivos BMP e dados de pixel e, em seguida, de passagem, que MemoryStream com o método ImageSource.FromStream. A classe BmpMaker na biblioteca Xamarin.FormsBook.Toolkit demonstra isso. Ele cria uma BMP na memória usando um pixel 32-bit formato-8 bits cada para vermelho, verde, azul e alfa (opacidade) Canais.

A classe BmpMaker foi codificado com o desempenho em mente, na esperança de que ele pode ser usado para animação. Talvez um dia ele vai ser, mas neste capítulo a única demonstração é um gradiente de cor simples.

O construtor cria uma matriz de byte buffer chamado que armazena toda a BMP começando com a informação de cabeçalho e seguido pelos bits de pixel. O construtor, em seguida, usa um MemoryStream para escrever as informações de cabeçalho no início deste buffer:

Depois de criar um objeto BmpMaker, um programa pode então chamar um dos dois métodos setPixel para definir a cor de cada linha e coluna particular. Ao fazer muitas chamadas, a chamada SetPixel que usa um valor Cor é significativamente mais lento do que aquele que aceita vermelho explícita, verde, e os valores azuis.

O último passo é chamar o método gerar. Este método instancia outra MemoryStream ob- jecto com base na matriz de buffer e usa-o para criar um objeto FileImageSource. Você pode ligar para gerarem várias vezes depois de definir novos dados de pixel. O método cria um novo MemoryStream cada vez, porque ImageSource.FromStream fecha o objeto Stream quando estiver terminado com ele.

O DiyGradientBitmap programação &quot;DIY&quot; significa &quot;faça você mesmo&quot; demonstrado como usar BmpMaker para fazer um mapa de bits com um gradiente simples e exibi-lo para preencher a página. A inclui arquivo XAML o elemento Image:

O arquivo code-behind instancia um Bmp Maker e percorre as linhas e colunas do mapa de bits para criar um gradiente que varia de vermelho na parte superior para azul na parte inferior:

Aqui está o resultado:

Agora use a sua imaginação e veja o que você pode fazer com BmpMaker.