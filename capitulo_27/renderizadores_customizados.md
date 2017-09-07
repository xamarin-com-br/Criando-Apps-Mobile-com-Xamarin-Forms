## Renderizadores Customizados {#renderizadores-customizados}

No centro do Xamarin.Forms está uma coisa que pode parecer mágica: a habilidade de um simples elemento como um Button aparecer como um botão nativo nos sistemas operacionais iOS, Android e Windows. Neste capítulo você verá como nessas três plataformas como cada elemento no Xamarin.Forms é suportado por uma classe especial conhecida como _renderer_. Por exemplo, a classe Button no Xamarin.Forms é suportada por várias classes nas diversas plataformas, cada nomeada ButtonRenderer.

A notícia boa é que você pode escrever seus próprios renderizadores, e neste capítulo lhe mostraremos como. Entretanto, lembre-se que renderizadores customizados é um tópico grande e este capítulo é apenas o começo.

Escrever um renderizador customizado não é tão fácil quanto escrever uma aplicação Xamarin.Forms. Você precisará estar familiarizado com as plataformas iOS, Android e Windows. Mas obviamente é uma técnica poderosa. De fato, alguns desenvolvedores pensão no valor final do Xamarin.Forms como fornecedor de um framework estruturado para escrever renderizadores customizados.