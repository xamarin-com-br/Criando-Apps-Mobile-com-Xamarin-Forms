## O Projeto Windows Phone {#o-projeto-windows-phone}

No projeto do Windows Phone, veja o arquivo MainPage.xaml.cs escondido debaixo do arquivo Main-Page.xaml na lista de arquivos de projeto. Esse arquivo define a classe MainPage habitual, mas perceba que ele deriva de uma classe Xamarin.Forms chamada FormsApplicationPage. A classe App recém-instanciada é passada para o método LoadApplication definido por esta classe base:

A configuração da propriedade SupportedOrientations permite que o telefone responda às mudanças de orientação entre retrato e paisagem. Os projetos iOS e Android são ativados para mudanças de orientação por padrão, bem, então você deve ser capaz de transformar o lado do telefone ou o simulador para o outro e ver o texto realinhados no centro da tela.