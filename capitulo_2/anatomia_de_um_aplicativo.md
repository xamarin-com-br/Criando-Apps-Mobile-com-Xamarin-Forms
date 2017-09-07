## Anatomia de um aplicativo {#anatomia-de-um-aplicativo}

A interface de usuário moderna é construída a partir de objetos visuais de vários tipos. Dependendo do sistema operacional, esses objetos virtuais podem ter diferentes nomes - controles, elementos, views, widgets - mas todos eles são dedicados aos trabalhos de apresentação ou interação.

Em Xamarin.Forms, os objetos que aparecem na tela são chamados coletivamente de _elementos visuais_. Eles vêm em três categorias principais:

*   Page
*   Layout
*   View

Estes não são conceitos abstratos! A interface de programação de aplicativo Xamarin.Forms (API) define classes nomeadas _VisualElement_, _Page_, _Layout_ e View. Estas classes e seus descendentes a partir da espinha dorsal da interface do Xamarin.Forms. _VisualElement_ é uma classe extremamente importante em Xamarin.Forms. Um objeto VisualElement é algo que ocupa espaço na tela.

Uma aplicação Xamarin.Forms consiste de uma ou mais páginas. Uma página normalmente ocupa toda (ou pelo menos uma grande área) da tela. Algumas aplicações são compostas por apenas uma única página, enquanto outros permitem navegar entre várias páginas. Em muitos dos primeiros capítulos deste livro, você vai ver apenas um tipo de página, chamada de _ContentPage_.

Em cada página, os elementos visuais são organizados em uma hierarquia pai-filho. O filho de um _ContentPage_ é geralmente um layout de algum tipo para organizar os elementos visuais. Alguns layouts tem um único filho, mas muitos layouts têm vários filhos que o layout organiza dentro de si. Estes filhos podem ser outros layouts ou views. Diferentes tipos de layouts de filhos providenciam uma pilha, num grid bidimensional, ou de um modo mais livre.

O termo _view_ em Xamarin.Forms indica tipos de objetos de apresentação e objetos interativos: texto, bitmaps, botões, campos de entrada de texto, controles deslizantes, interruptores, barras de progresso, data e hora, e outros de sua própria invenção. Estes são muitas vezes chamados de controles ou widgets em outros ambientes de programação. Este livro se refere a eles como _views_ ou _elements_. Neste capítulo, você encontrará o _Label_ para mostrar no texto.