Frontend: React.js (JavaScript puro), Tailwind CSS (para layout e sidebar slide-in), js-draw (motor principal de desenho e anotação vetorial).

Backend: Node.js, Express.js (API REST para gerenciar persistência de dados), Multer (gerenciamento de upload de PDFs e imagens).

Fase 1: Configuração do Backend (Node.js)

Inicializar o servidor Express e configurar o middleware CORS e JSON.

Criar rotas para upload e armazenamento de arquivos estáticos:

POST /api/upload/pdf: Salva arquivos PDF enviados na aba Cadernos.

POST /api/upload/image: Salva imagens adicionadas aos quadros de desenho.

Endpoints CRUD para persistir o estado dos desenhos e anotações dos usuários.

Fase 2: Layout Base e Navegação (React)

Desenvolver o componente principal dividindo a tela em duas áreas:

Atualização da Arquitetura e Layout Responsivo (React)

Sidebar Adaptativa (Slide-in/Slide-out):

Em Tablets (Prioridade): O menu lateral funciona como um drawer flutuante com sobreposição (overlay) e efeito de desfoque de fundo (backdrop-blur), acionado por um gesto de toque a partir de um botão na borda esquerda preservando a máxima área útil de desenho.

Em Desktops: A sidebar pode transicionar para um formato fixo colapsável lado a lado para aproveitar telas panorâmicas.

Gerenciamento de Estado de Toque no Canvas: Implementar um ganho intermediário em React para alternar dinamicamente o comportamento do canvas entre modo de visualização/navegação (dedos) e modo de criação (stylus), prevenindo conflitos comuns em telas touch.

Área de Trabalho Principal: Onde o componente ativo (quadro livre ou visualizador de PDF) será renderizado.

Fase 3: Barra de Tarefas, Ferramentas de Traço e Roda de Cores Copic
1. Seletor de Cores em Roda e Sistema Copic Sketch (358 Cores)
Base de Dados Copic: Criar um arquivo estruturado no projeto (src/constants/copicColors.js) contendo o mapeamento das 358 cores oficiais da Copic Sketch, divididas por suas famílias clássicas (ex: BV - Blue Violet, B - Blue, BG - Blue Green, G - Green, YG - Yellow Green, Y - Yellow, YR - Yellow Red, R - Red, RV - Red Violet, V - Violet, E - Earth, W - Warm Gray, C - Cool Gray, N - Neutral Gray, T - Toner Gray), associando o código Copic (ex: R27, E00, B000) ao seu respectivo código HEX.

Interface da Roda de Cores (Color Wheel):

Na barra de tarefas superior, ao clicar no ícone de mudança de cor, abrir um popover flutuante contendo uma Roda de Cores Interativa (desenvolvida via canvas HSV ou utilizando componentes validados como react-color).

Modo Duplo de Seleção:

Modo Livre (Wheel): O usuário arrasta o seletor na roda para escolher qualquer tom RGB/HEX livremente.

Modo Copic Exato: Uma gaveta ou aba secundária na mesma janela flutuante que exibe as famílias Copic em blocos organizados, permitindo escolher a cor exata pelo código (ex: selecionar exatamente o tom de pele E04 ou o cinza C3).

Algoritmo de Aproximação (Snap-to-Copic): Opcionalmente, ao escolher uma cor livre na roda, o sistema pode calcular a cor Copic Sketch mais próxima usando a distância Euclidiana no espaço RGB e exibir o código Copic correspondente na interface.


Fase 4: Módulo de Cadernos (PDFs e Stylus)

Integrar uma biblioteca de renderização de PDF (como pdfjs-dist) para converter páginas do arquivo em camadas visuais.

Sobrepor a instância do js-draw diretamente sobre a camada do PDF renderizado, garantindo suporte total a dispositivos touch, mouse e canetas stylus (com detecção de pressão quando suportada pelo navegador).

Implementar controles de paginação (próxima página / página anterior) para navegar pelo caderno mantendo as anotações vinculadas a cada página específica.

Fase 5 (Nova): Quadro Virtualmente Infinito (Aba Desenho)Para atender ao requisito de que a aba Quadros cresça automaticamente conforme o usuário desenha próximo às bordas, a arquitetura deve gerenciar o espaço de forma dinâmica:Estratégia de Renderização no js-draw:Configurar o canvas do js-draw sem restrições rígidas de tamanho de página fixa (infinite canvas mode).  Expansão Dinâmica por Coordenadas (Bounding Box Monitoring):Criar um ganho (hook) no React que escuta os eventos de desenho em tempo real do js-draw (onPointerDown, onDraw).Monitorar as coordenadas extremas ($X_{min}, X_{max}, Y_{min}, Y_{max}$) dos traços criados.Sempre que o usuário se aproximar de uma das margens atuais (por exemplo, a menos de 150px da borda visível), o contêiner do canvas expande programaticamente suas dimensões virtuais (ex: adicionando mais 1000px à direção correspondente) e reajusta o deslocamento da câmera (pan) suavemente para evitar cortes secos.Fundo Infinito (Grid/Pontilhado Dinâmico):Aplicar um padrão de fundo repetitivo em CSS (CSS Background Pattern com pontos ou malha quadriculada) que se desloca junto com o movimento de panorâmica (panning) do usuário, dando a sensação contínua de um caderno infinito (estilo OneNote ou Miro).



