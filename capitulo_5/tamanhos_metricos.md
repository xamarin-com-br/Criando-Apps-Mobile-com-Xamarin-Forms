## Tamanhos métricos {#tamanhos-m-tricos}

Aqui, novamente, são as relações subjacentes assumidas entre unidades e polegadas independentes de dispositivo nas três plataformas:

*   **iOS: 160 unidades por polegada**
*   **Android: 160 unidades por polegada**
*   **Windows Phone: 240 unidades por polegada**

Se o sistema métrico é mais confortável para você, aqui estão os mesmos valores para centímetros (arredondado para facilitar memorização e números facilmente divisíveis):

*   **iOS: 64 centímetros**
*   **Android: 64 centímetros**
*   **Windows Phone: 96 centímetros**

Isto significa que as aplicações Xamarin.Forms pode dimensionar um objeto visual em termos de dimensão métrica que é, em unidades familiares de polegadas e centímetros. Aqui é um programa chamado MetricalBoxView que exibe uma BoxView com uma largura de aproximadamente um centímetro e uma altura de cerca de uma polegada:

public class MetricalBoxViewPage: ContentPage

{ public MetricalBoxViewPage()

{

Content = new BoxView

{

Color = Color.Accent,

WidthRequest = Device.OnPlatform(64, 64, 96),

HeightRequest = Device.OnPlatform(160, 160, 240),

HorizontalOptions = LayoutOptions.Center,

VerticalOptions = LayoutOptions.Center

};

}

}

Se você atualizar uma régua para o objeto na tela do seu telefone, você vai descobrir que não é exatamente o tamanho desejado, mas certamente próximo a ele.