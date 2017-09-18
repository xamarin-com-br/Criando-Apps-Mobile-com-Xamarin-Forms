# 

## Estilo básico

No capítulo 10, "extensões de marcação XAML", você viu um trio de botões que continha uma grande quantidade de  marcação idêntica. Aqui estão eles novamente:

![](/assets/12-1-estilo.png)

![](/assets/12-2-estilo.png)

![](/assets/12-3-estilo.png)

Com exceção da propriedade **Text**, todos os três botões têm as mesmas configurações de propriedades.

Uma solução parcial para essa marcação repetitiva envolve a definição de valores de propriedade de um recurso dicionário e referenciá-los com a extensão de marcação "**StaticResource**". Como você viu no projeto "**ResourceSharing**"  no Capítulo 10, esta técnica não reduz o volume de marcação, mas ela consolida os valores em um só lugar. Para reduzir o volume de marcação, você vai precisar de um estilo. Um objeto de estilo é quase sempre definido em um "**ResourceDictionary**". Geralmente, você vai começar com uma seção de recursos na parte superior da página:

![](/assets/12-04REsourceDictionary.png)![](/assets/12-05-resorcedictionay1.png)Instancie o estilo com tags de inicio e fim separados.

![](/assets/12-06-estilo-tags.png)

