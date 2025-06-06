Detecção de Contornos com Processamento de Imagens
Este projeto explora o uso da biblioteca OpenCV, combinada com Python, para aplicar técnicas variadas de processamento de imagem com o intuito de localizar e destacar os principais contornos de objetos em diferentes tipos de imagens.

Finalidade
O script main.py foi desenvolvido para carregar três imagens específicas — GIRAFA.jpeg, SATELITE.jpeg e AVIAO.jpeg — e aplicar um fluxo de tratamento personalizado para cada uma, com o objetivo de isolar e evidenciar o objeto central presente nelas.

Estratégias Utilizadas
Como cada imagem possui características visuais únicas — variações de contraste, iluminação ou fundo —, o script adota duas estratégias principais de processamento:

Limiarização (Thresholding)

Detecção de Bordas com Canny

A escolha da técnica depende da imagem, sendo definida por parâmetros ajustáveis no próprio código.

1. Técnica de Thresholding
Usada nas imagens GIRAFA.jpeg e SATELITE.jpeg, essa abordagem busca separar o objeto principal do plano de fundo com base nos níveis de intensidade dos pixels.

Etapas do processo:

Conversão de Formato:
A imagem é inicialmente lida no formato BGR, convertida para RGB (para compatibilidade com o Matplotlib) e depois para tons de cinza, já que o thresholding funciona melhor em uma única faixa de intensidade.

Desfoque Gaussiano:
Aplica-se um filtro Gaussiano para suavizar a imagem, o que ajuda a reduzir ruídos e evitar a detecção de contornos irrelevantes.

Aplicação do Threshold:

Global (Otsu): Ideal para imagens como a da girafa, onde o contraste entre objeto e fundo é nítido. O método Otsu encontra automaticamente um valor de corte adequado.

Adaptativo: Preferido para a imagem do satélite, que possui iluminação irregular. O threshold adaptativo calcula valores locais com base na vizinhança de cada pixel, permitindo uma segmentação mais precisa.

Tratamento Morfológico:
Operações como abertura (MORPH_OPEN) são utilizadas para eliminar ruídos remanescentes na imagem binarizada, especialmente em áreas pequenas e isoladas.

Extração de Contornos:
Utiliza-se cv2.findContours com o modo RETR_EXTERNAL para obter apenas os contornos externos das formas brancas resultantes do threshold.

Seleção e Desenho:
Após extrair os contornos:

Para a girafa, seleciona-se o maior contorno (provavelmente o corpo do animal).

Para o satélite, os três maiores contornos são desenhados, aumentando as chances de capturar o objeto mesmo que ele não seja o mais dominante em área.

2. Técnica de Bordas com Canny
Empregada na imagem do avião (AVIAO.jpeg), essa abordagem foi necessária devido à complexidade visual do fundo, onde a limiarização não funcionou bem.

Etapas envolvidas:

Pré-processamento:
Assim como na outra técnica, a imagem passa por conversões de cor e suavização com filtro Gaussiano.

Detecção com Canny:
O algoritmo cv2.Canny é usado para identificar bordas baseando-se nos gradientes de intensidade. Dois limiares controlam a sensibilidade: um para bordas fortes e outro para fracas.

Dilatação das Bordas:
As bordas detectadas são dilatadas com cv2.dilate para conectá-las melhor, criando contornos mais contínuos e fáceis de identificar.

Extração e Filtro de Contornos:
Após detectar os contornos:

Filtram-se os muito pequenos (área mínima).

Ignoram-se contornos localizados muito próximos à base da imagem (assumindo que o avião está mais ao centro ou no topo).

Desenho Final:
Os contornos que passam nos filtros são desenhados na imagem original, destacando as bordas relevantes do avião.

Parâmetros Personalizados
O script define um dicionário chamado parametros_por_imagem, onde cada imagem possui um conjunto de parâmetros personalizados:

Tipo de abordagem ('threshold' ou 'canny')

Tamanhos de kernel

Níveis de limiar

Configurações para operações morfológicas ou dilatação

Filtros para selecionar contornos relevantes

Esse sistema permite calibrar o processamento para cada imagem individualmente, garantindo maior precisão na detecção.

Como Executar
Para rodar o script e visualizar os resultados, utilize:

bash
Copy
Edit
python lab06/main.py
Pré-requisitos:

Certifique-se de que as bibliotecas necessárias estão instaladas:

bash
Copy
Edit
pip install opencv-python numpy matplotlib
E mantenha as imagens no caminho lab06/figs.
