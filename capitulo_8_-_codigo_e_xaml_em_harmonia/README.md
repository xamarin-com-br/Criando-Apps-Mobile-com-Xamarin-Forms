# Capítulo 8 - Código e XAML em harmonia {#cap-tulo-8-c-digo-e-xaml-em-harmonia}

O arquivo de código e o arquivo XAML sempre existem em pares. Ambos os arquivos se complementam. Apesar de ser referido como &quot;_code-behind_&quot; para o arquivo de código de um arquivo XAML, muitas vezes o código é proeminente em assumir as partes mais ativas e interativas da aplicação. Isso implica que o _code-behind_ deve ser capaz de referenciar os elementos definidos em XAML com objetos facilmente instanciados no código. Da mesma forma, os elementos do XAML devem ser capaz de ativar os eventos que possam ser manipulados pelo arquivo código. Isso é oque veremos neste capítulo.

Mas primeiro, vamos explorar algumas técnicas incomuns para instanciar objetos em um arquivo XAML.