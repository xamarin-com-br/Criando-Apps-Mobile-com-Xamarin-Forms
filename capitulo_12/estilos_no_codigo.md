## Estilos no código {#estilos-no-c-digo}

Embora os estilos são mais os mais definidos e usados em XAML, você deve saber como eles se parecem quando definidos e usados no código. Aqui está a classe de página para o projeto "**BasicStyleCode**" somente de código. O construtor da classe "**BasicStyleCodePage**" usa a sintaxe objeto de inicialização para imitar a sintaxe "XAML" na definição do objeto Estilo e aplicá-lo para três botões:

![](/assets/12-16-EstiloNoCodigo.png)

![](/assets/12-16-EstiloNoCodiogo1.png)



É muito mais evidente no código que em "**XAML**" a propriedade "**property**" do "**Setter**" é do tipo "**BindableProperty**".

Os dois primeiros objetos "**Setter**" neste exemplo são inicializados com os objetos "**BindableProperties**" nomeados "**View.HorizontalOptionsProperty**" e "**View.VerticalOptionsProperty**". Você poderia usar "**Button.HorizontalOptionsProperty**" e "**Button.VerticalOptionsProperty**" em vez disso porque "**Button**" herda essas propriedades de "**View**". Ou você poderia mudar o nome da classe para qualquer outra classe que deriva de "**View**".

Como de costume, o uso de um "**ResourceDictionary**" no código parece inútil. Você poderia eliminar o dicionário e apenas atribuir os objetos de estilo diretamente para as propriedades de estilo dos botões. Contudo, mesmo em código, o estilo é uma maneira conveniente de agrupar todas as configurações de propriedade juntas em um pacote compacto.

