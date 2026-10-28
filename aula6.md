## Detecção de bordas: encontrando os contornos de uma imagem

Pegue uma caneta e desenhe um gato com poucos traços: o contorno das orelhas, das costas e do rabo. Não precisa de cor, sombra ou textura, e qualquer pessoa reconhece o gato. Os contornos carregam uma parte enorme da informação de uma imagem, e o nosso cérebro sabe disso. No fim dos anos 1950, Hubel e Wiesel descobriram que certos neurônios do córtex visual reagem a bordas em orientações específicas, uma descoberta que rendeu a eles o Nobel de 1981. Décadas depois, aconteceu algo curioso: redes neurais treinadas para reconhecer objetos passaram a aprender, sozinhas, filtros de borda nas primeiras camadas. Ninguém mandou. Elas descobriram que começar pelas bordas funciona.

O problema é que o computador não vê o gato. Ele vê uma matriz de números, e nessa matriz não existe nada chamado “contorno”. Existe só um valor ao lado de outro. Detectar bordas é justamente transformar essa tabela em algo parecido com o desenho a caneta.

Borda é salto, não objeto

A pista que o computador usa é a mudança brusca de intensidade. Onde os valores saltam de repente, provavelmente há uma fronteira. Só que essa fronteira nem sempre é o limite de um objeto. Pode ser uma troca de cor na mesma superfície, uma dobra que muda o ângulo em relação à luz ou uma sombra projetada no chão. Para nós, uma sombra é obviamente diferente de um objeto. Para o computador, as duas são o mesmo salto numérico. Guarde essa ideia, porque ela volta no final.

Medindo o salto com derivadas

Se a imagem é uma função de intensidade, medir mudança é calcular a derivada. Numa imagem digital, isso se resume a subtrair vizinhos. Veja a sequência:

20, 20, 20, 180, 180

Subtraindo cada valor do seguinte, obtemos 0, 0, 160, 0. Onde nada muda, o resultado é zero. Onde há o salto do escuro para o claro, aparece 160. Indo no sentido contrário, o sinal seria negativo, e o tamanho do número diz o quão forte é a transição.

Uma imagem tem duas dimensões, então medimos a variação na horizontal e na vertical. Juntas, essas duas medidas formam o gradiente. Uma boa forma de visualizar é imaginar a imagem como um terreno, em que o brilho de cada pixel é a altitude. Regiões uniformes são planícies, e uma borda é um penhasco. Em cada ponto, o gradiente é uma seta que aponta para a subida mais íngreme, e o tamanho dessa seta mostra o quão íngreme ela é. Por isso o gradiente é sempre perpendicular à borda: ele aponta para quem vai subir o penhasco, não para quem caminha ao longo dele.

Pequenas máscaras, grandes nomes

Na prática, essas derivadas são calculadas com pequenas máscaras que percorrem a imagem, e cada uma tem sua história.

O operador de Roberts, de 1963, usa máscaras 2×2 e mede as diagonais. Ele vem da tese de doutorado de Lawrence Roberts no MIT, um dos trabalhos fundadores da visão computacional. Roberts depois trocou de área e foi um dos arquitetos da ARPANET, a precursora da internet.

Prewitt e Sobel usam máscaras 3×3 e medem na horizontal e na vertical. O detalhe que faz diferença no Sobel são os pesos 1, 2, 1, com o centro mais forte. Enquanto deriva numa direção, ele suaviza um pouco na outra, o que o deixa mais resistente ao ruído. O Sobel nasceu numa palestra em Stanford em 1968 e não virou artigo na época. Mesmo assim, se espalhou e virou padrão em praticamente toda biblioteca de imagens.

O outro caminho: a segunda derivada

Também dá para olhar a segunda derivada, usando o operador Laplaciano. Numa borda, a primeira derivada forma um pico, e a segunda troca de sinal exatamente no topo desse pico. Esse ponto é o cruzamento por zero. A transição é a mesma, mas a pergunta muda: em vez de “onde a mudança é forte?”, passa a ser “onde a mudança para de crescer?”.

O Laplaciano responde igualmente a bordas em qualquer direção com uma única conta. Em troca, não diz qual é a direção da borda e é ainda mais sensível ao ruído. Foi dessa linha que saiu o detector de Marr e Hildreth, em 1980. Ele usa o Laplaciano da Gaussiana, conhecido como “chapéu mexicano” pelo formato, e foi um dos antecessores diretos do Canny.

O vilão: ruído

Aqui tudo complica. O ruído também cria diferenças grandes entre pixels vizinhos, e a derivada amplifica isso sem piedade. Ruído é variação rápida, e a derivada é um medidor de variação rápida. Aplicada direto na imagem, ela acende bordas onde só existe granulado.

A saída é suavizar antes de derivar, com um filtro gaussiano. Existe ainda um atalho elegante: suavizar e depois derivar dá o mesmo resultado que aplicar direto a derivada da gaussiana. É um filtro só, numa passada só.

Mas suavizar tem um preço, controlado pelo sigma da gaussiana. Com sigma pequeno, os detalhes finos ficam, e o ruído também. Com sigma grande, a imagem fica limpa, mas detalhes somem e as bordas perdem a posição exata. É o mesmo cabo de guerra da filtragem espacial, agora com outra cara.

O que fica

Lembra da sombra lá do começo? O computador não sabe o que é um objeto. Ele só enxerga números mudando, e cabe a nós dar critérios para separar o contorno que importa do ruído, da textura e da sombra. O gradiente já dá um bom passo: ele encontra as bordas. Mas entrega contornos grossos, borrados e cheios de sujeira, muito longe do desenho a caneta. Transformar isso num traço fino e limpo é o trabalho do detector de Canny, assunto do próximo post.
