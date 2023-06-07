[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/jOwtmjT8)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-718a45dd9cf7e7f842a935f5ebbe5719a5e09af4491e668f4dbf3b35d5cca122.svg)](https://classroom.github.com/online_ide?assignment_repo_id=11269108&assignment_repo_type=AssignmentRepo)
# INF0429 - Percepcao e Acao Robotica
## A01 - Primeira Prova

Matricula - 202005478
Nome completo - Gabriel Urzeda

## Relatório - Simulador Cinemático de um Robô de Tração Diferencial

Este código Python implementa um simples simulador de um robô de tração diferencial, que é um tipo de robô móvel que possui duas rodas acionadas independentemente e um eixo comum. Neste relatório, iremos analisar cada parte do código e sua funcionalidade.

### Classe DifferentialDriveRobot

A classe `DifferentialDriveRobot` é usada para representar o robô de tração diferencial. A classe tem dois atributos de instância: `wheel_radius` e `wheel_distance`, que representam o raio das rodas e a distância entre elas, respectivamente. A classe também possui um atributo de estado (`state`), que é um vetor numpy de três elementos que contém a posição x e y e a orientação do robô (theta).

O método `move` na classe `DifferentialDriveRobot` é usado para mover o robô com uma velocidade linear e angular específica por um tempo `dt`. Primeiro, ele verifica se `dt` é um número positivo. Depois, ele calcula a alteração do estado no frame do robô e usa a matriz de rotação para transformar essa alteração para o frame inercial. O estado atual do robô é então atualizado com essa alteração.

O método `compute_odometry` simplesmente retorna uma cópia do estado atual do robô.

### Função simulate

A função `simulate` é usada para simular o movimento do robô com uma velocidade linear e angular específica por um tempo `dt` para um número máximo de iterações (`max_iter`). Ela primeiro inicializa uma lista vazia para armazenar a trajetória do robô. Em seguida, em um loop for, move o robô, calcula a odometria e adiciona a odometria à lista de trajetórias. A função então retorna a trajetória como um array numpy.

### Função plot_trajectory

A função `plot_trajectory` é usada para plotar a trajetória do robô. Ela cria um gráfico onde o eixo x representa a posição x do robô e o eixo y representa a posição y do robô. A trajetória do robô é plotada em azul, enquanto a orientação do robô em cada passo é representada por uma seta vermelha.

### Simulação e Plotagem

Na parte final do código, um objeto `DifferentialDriveRobot` é criado e os parâmetros de controle para a simulação são definidos. A função `simulate` é então chamada para simular o movimento do robô, e a trajetória resultante é plotada usando a função `plot_trajectory`.

### Conclusão

Este script fornece uma implementação básica de um simulador cinemático para um robô de tração diferencial. Ele permite simular o movimento do robô com uma velocidade linear e angular específica e visualizar a trajetória resultante. Enquanto este script é bastante simples, ele serve como uma base sólida para a simulação de movimentos mais complexos e para o teste de algoritmos de controle e planejamento de trajetória.

## Relatório - Simulador Cinemático de Robô em ROS2

## Descrição do problema

O código implementado visa controlar um robô em um ambiente de simulação e visualizar os movimentos do robô com base nos dados coletados. O objetivo é controlar o movimento do robô, que possui uma configuração de acionamento diferencial, permitindo a ele mover-se para frente/trás e girar, e depois gerar uma representação visual desse movimento.

## Solução Implementada

A solução envolve a criação de um nó ROS2, `DifferentialDriveSim`, que assina a mensagem do tipo `Twist` para receber comandos de velocidade e publica a posição atual e a orientação do robô como uma mensagem de `Odometry`.

Em seguida, esses dados são coletados e salvos em um arquivo txt, que é posteriormente usado para gerar um gráfico 3D dos movimentos do robô.

### Descrição das funções:

- **\_\_init\_\_**: Esta é a função inicializadora para a classe `DifferentialDriveSim`. Ela inicializa o publicador e o assinante necessários, configura o timer, e prepara outros parâmetros necessários.

- **velocity_callback**: Esta função é chamada sempre que uma mensagem de velocidade é recebida. Ela atualiza a posição e a orientação do robô com base na velocidade linear e angular recebida.

- **publish_odometry**: Esta função é chamada em intervalos regulares (definidos pela variável `timer_period`). Ela publica a posição e orientação atuais do robô como uma mensagem `Odometry`.

- **teleop**: Esta função permite o controle manual do robô através do teclado. Ela contém um loop que verifica constantemente a entrada do teclado e move o robô de acordo.

- **perform_autonomous_movement**: Esta função controla o robô para executar um conjunto predefinido de movimentos.

- **getKey**: Esta função é usada para obter a entrada do teclado.

- **save_velocity_callback_data**: Esta função salva os dados coletados na função `velocity_callback` em um arquivo `.txt`.

- **plot_movement_data**: Esta função lê os dados coletados do arquivo `.txt` e gera um gráfico 3D dos movimentos do robô. Além disso, ela cria um vídeo que mostra o movimento do robô ao longo do tempo.

- **main**: Esta função inicializa o nó ROS2 e inicia o controle do robô.

## Execução do Código

### Instruções para executar o código em um ambiente Docker com ROS2:

1. Certifique-se de ter o Docker instalado no seu sistema. Se ainda não tiver, você pode baixá-lo [aqui](https://docs.docker.com/get-docker/).

2. Certifique-se de que a imagem do Docker está devidamente construída com todas as dependências necessárias para o ROS2 e o Gazebo.

3. Inicie o ROS2 e teste seu teleop usando o seguinte comando:

    ```bash
    source ./docker_gazebo.sh 
    ros2 launch albot-description sim.launch.py
    ```

4. Abra outro terminal.

5. Verifique os contêineres Docker em execução usando o comando:

    ```bash
    docker ps
    ```

6. Acesse o contêiner Docker onde

 o ROS2 está rodando usando o seguinte comando. Substitua `<ContainerID>` pelo ID do contêiner Docker que você quer acessar:

    ```bash
    docker exec -it <ContainerID> bash
    ```

7. Uma vez dentro do contêiner, execute o pacote ROS:

    ```bash
    ros2 run bot my_teleop
    ```

8. Depois de executar o código e coletar os dados de posição e orientação do robô, você pode recuperar o arquivo txt gerado (`velocity_callback_data.txt`) a partir do contêiner Docker. Substitua `<ContainerID>` pelo ID do contêiner Docker que você quer acessar e `<Caminho que desejar salvar o arquivo>` pelo caminho na sua máquina local onde você quer que o arquivo seja salvo.

    ```bash
    sudo docker cp <ContainerID>:/gazebo_ws/velocity_callback_data.txt <Caminho que desejar salvar o arquivo>
    ```

Agora, você deve ter o arquivo `velocity_callback_data.txt` na localização que você especificou.

9. Finalmente, você pode visualizar os movimentos do robô gerando um gráfico 3D e um vídeo com o seguinte comando:

Para executar o script `plot_movement_data.py`, é necessário fornecer o caminho do arquivo txt que contém os dados do movimento do robô. Faça isso adicionando o caminho do arquivo na variável `data_file_path` no código, como mostrado abaixo:

```python
# Caminho do arquivo de dados
data_file_path = '/path/to/your/file/velocity_callback_data.txt'
```

Substitua `'/path/to/your/file/velocity_callback_data.txt'` pelo caminho do arquivo txt no seu sistema.


Depois disso, você pode executar o script `plot_movement_data.py` como um programa Python normal. No terminal, navegue até o diretório que contém o script e execute o seguinte comando:

```bash
python3 plot_movement_data.py
```

Certifique-se de que você tem as bibliotecas necessárias (`numpy`, `matplotlib`, `mpl_toolkits.mplot3d`, `cv2`) instaladas. Caso contrário, você pode instalá-las usando pip:

```bash
pip install numpy matplotlib opencv-python
```

`plot_movement_data.py`, é responsável por visualizar os dados da trajetória do robô. Ele primeiro lê os dados do arquivo, depois plota os dados em um gráfico 3D e finalmente cria um vídeo mostrando a trajetória do robô ao longo do tempo.

O código começa importando as bibliotecas necessárias:

- `numpy`: usada para manipulação de dados.
- `matplotlib.pyplot` e `mpl_toolkits.mplot3d`: usadas para a plotagem de gráficos.
- `cv2`: usada para a criação de vídeos.

Em seguida, o caminho do arquivo que contém os dados da trajetória é definido na variável `data_file_path`.

Os dados são lidos do arquivo usando `numpy.genfromtxt`, que lê um arquivo de texto com dados delimitados e retorna uma matriz numpy. 

Os dados são então divididos em dados positivos e negativos com base no valor da terceira coluna (índice 2), que representa o movimento ao longo do eixo Z. Isso é feito usando a indexação booleana, uma característica do numpy que permite selecionar elementos de uma matriz com base em condições.

A plotagem do gráfico 3D é feita usando matplotlib e mpl_toolkits. Primeiro, é criada uma figura e um subplot com projeção 3D. Em seguida, os dados positivos e negativos são plotados em cores diferentes para indicar a direção do movimento do robô. A legenda é adicionada para explicar as cores.

O gráfico é então salvo como uma imagem usando `fig.savefig`.

Para criar o vídeo, o tamanho do frame e a taxa de quadros por segundo (fps) são definidos. Em seguida, é criado um objeto VideoWriter usando `cv2.VideoWriter`.

O loop for percorre todos os dados e para cada ponto, limpa a figura, plota os dados até o ponto atual e adiciona uma legenda. O gráfico é então convertido em uma imagem usando a função `canvas.draw` do matplotlib e a função `np.frombuffer`. A imagem é redimensionada para o tamanho do frame e escrita no vídeo várias vezes para criar a ilusão de movimento rápido.

Finalmente, o vídeo é fechado usando `out.release` e todas as figuras são fechadas com `plt.close('all')`.

Depois de executar o script, você deve ver um gráfico 3D do movimento do robô e um vídeo do movimento ao longo do tempo. A imagem do gráfico será salva como `grafico_3D.png` e o vídeo será salvo como `movimento.mp4` na mesma pasta.

## Justificativa da Implementação

A implementação usa o ROS2 para a comunicação entre os diferentes componentes do sistema. ROS2 é uma escolha popular para sistemas robóticos devido à sua eficiência na comunicação interprocessos e sua grande base de usuários.

O código é dividido em funções para facilitar o entendimento e a manutenção. Além disso, são implementados tanto o controle manual quanto o autônomo, para fornecer flexibilidade no uso do robô.

A escolha de salvar os dados coletados em um arquivo txt permite fácil recuperação e análise dos dados posteriormente. A geração de um gráfico 3D e de um vídeo permite uma representação visual intuitiva dos movimentos do robô.

## Limitações e Possíveis Melhorias

A implementação atual assume um robô com acionamento diferencial e pode não funcionar corretamente para outros tipos de robôs. Para uma solução mais genérica, pode-se considerar a implementação de diferentes modelos de controle para diferentes tipos de robôs.

Além disso, o movimento autônomo é pré-definido e pode não funcionar bem em todos os ambientes. Uma possível melhoria seria a implementação de algoritmos de planejamento de trajetória ou de aprendizado por reforço para permitir que o robô navegue de forma autônoma em diferentes ambientes.