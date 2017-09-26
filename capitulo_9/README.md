# Capítulo 9 {#cap-tulo-9}

## Chamadas de API específicas da plataforma {#chamadas-de-api-espec-ficas-da-plataforma}

Uma emergência surgiu. Qualquer um que joga **MonkeyTap **do capítulo anterior vai chegar rapidamente à conclusão de que ele precisa desesperadamente de uma melhoria muito simples, e ele simplesmente não pode viver sem ela.

MonkeyTap precisa de som .

Ele não precisa de sons sofisticados apenas pequenos sinais sonoros para acompanhar os flashes dos quatro elementos **BoxView**. Mas a API Xamarin.Forms não suporta som, o som não é algo que podemos adicionar ao **MonkeyTap **com apenas um par de chamadas de API. Para facilitar suporte a som no Xamarin.Forms é necessário utilizar uma plataforma especifica de cada plataforma. Descobrir como fazer sons em IOS, Android e Windows Phone é bastante difícil. Mas como é que o programa Xamarin.Forms consegue fazer chamadas para as plataformas individuais?

Antes de abordar as complexidades de som, vamos examinar as diferentes abordagens para fazer chamadas de API específicas da plataforma com um exemplo muito mais simples: Os três primeiros programas curtos mostrado abaixo fazem a mesma coisa: todos eles exibirão dois pequenos itens de informações fornecidas pela plataforma subjacente do sistema operacional que irá revelar o modelo do dispositivo executando o programa e a versão do sistema operacional.

