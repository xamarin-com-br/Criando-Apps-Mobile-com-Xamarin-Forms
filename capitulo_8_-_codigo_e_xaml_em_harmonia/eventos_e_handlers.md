## Eventos e Handlers {#eventos-e-handlers}

Quando você toca em um **Buttom **do Xamarin.Forms, ele dispara um evento **Clicked**. Você pode instanciar um botão em XAML, mas o próprio manipulador de eventos **Clicked **deve residir no arquivo code-behind. O **Button **é apenas um de um grupo de _views_ que existem essencialmente para gerar eventos, de modo que o processo de tratamento eventos é crucial para coordenar arquivos XAML e códigos.

Anexar um manipulador de eventos para um evento em XAML é tão simples como definir uma propriedade; é, de fato, visivelmente indistinguível de uma definição da propriedade. O projeto **XamlKeypad **é uma versão XAML do projeto **PersistentKeypad **do Capítulo 6. Ela ilustra a configuração de  _**handlers**_** **em XAML e lidar com esses eventos no arquivo _code-behind_. Ele também inclui a lógica para salvar entradas de teclado quando o programa é terminado.

Se você der uma olhada novamente no código do construtor das classes **SimplestKeypadPage **ou **PersistentKeypadPage**, você verá um par de repetições para criar os botões que compõem a parte numérica do teclado. Claro, isso é precisamente o tipo de coisa que você não pode fazer em XAML, mas dá para perceber o quanto mais limpa é a marcação no **XamlKeypadPage **quando comparado com o código:

![](/assets/08-20-xamlkeypage)![](/assets/08-20-xamlkeypage2)

O arquivo é muito mais curto do que teria sido se as três propriedades em cada botão numérico fossem formatado em três linhas, entretanto, empacotar todas juntas faz a uniformidade da marcação muito mais óbvia e proporciona clareza em vez de obscuridade.

A grande questão é qual você preferiria manter e modificar: o código no **SimplestKeypadPage **ou construtores **PersistentKeypadPage **ou a marcação no arquivo **XamlKeypadPageXAML**?

Aqui está a captura de tela. Você vai ver que essas chaves estão agora organizadas em ordem da calculadora em vez de ordem de telefone:![](/assets/08-21-telas)

O botão **Backspace** tem seu evento **Clicked **definido como o manipulador **OnBackspaceButtonClicked**, enquanto os botões numéricos compartilham o manipulador **OnDigitButtonClicked**_._ Como você deve se lembrar, a propriedade StyleId é muitas vezes usado para distinguir **views **que compartilham o mesmo manipulador de evento, o que significa que os dois manipuladores de eventos podem ser implementados no arquivo _code-behind_ exatamente como os programas só de código:

![](/assets/08-22-code)![](/assets/08-22-code2)

Parte do trabalho do método **LoadFromXaml **chamado por **InitializeComponent **envolve anexar esses _**event** **handlers**_** **para os objetos instanciados a partir do arquivo XAML.

O projeto **XamlKeypad i**nclui o código que foi adicionado à página e as classes de aplicativos no **PersistentKeypad **para salvar o texto teclado quando o programa é encerrado. A classe App em **XamlKeypad **é basicamente a mesma que aquela em **PersistentKeypad**.

