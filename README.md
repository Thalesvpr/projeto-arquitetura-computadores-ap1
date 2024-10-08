# Projeto de Controle de Dispositivos com Arduino

Este projeto utiliza um microcontrolador Arduino para controlar dispositivos eletrônicos, como LEDs, um buzzer e sensores, por meio de comandos seriais e switches binários. O código é estruturado para responder a diferentes comandos e monitorar a temperatura e distância. A seguir, está uma explicação detalhada do funcionamento do código, bem como uma tabela de comandos e funções.

## Estrutura do Código

O código é dividido em várias seções principais:

### 1. Configuração de Pinos
No início do código, definimos os pinos que serão utilizados. Cada pino é associado a um dispositivo específico. Por exemplo, pinos para LEDs, um buzzer e um sensor DHT11.

```cpp
#define LED_A 2
#define LED_B 3
#define LED_C 4
#define BUZZER 12
#define RGB_PIN_RED 9
#define RGB_PIN_GREEN 10
#define RGB_PIN_BLUE 11
#define BINARY_SWITCH_1 5
#define BINARY_SWITCH_2 6
#define BINARY_SWITCH_3 7
#define BINARY_SWITCH_4 8
#define TRIG_PIN 40
#define ECHO_PIN 41
#define DHTPIN 35
#define DHTTYPE DHT11
#define ENTER_SWITCH 30
#define MODE_SWITCH 13
DHT dht(DHTPIN, DHTTYPE);
```

### 2. Inicialização
A função `setup()` é chamada uma vez quando o programa começa. Nela, os pinos são configurados como entrada ou saída e o sensor de temperatura é inicializado.

```cpp
void setup() {
    pinMode(LED_A, OUTPUT);
    pinMode(LED_B, OUTPUT);
    pinMode(LED_C, OUTPUT);
    pinMode(BUZZER, OUTPUT);
    pinMode(RGB_PIN_RED, OUTPUT);
    pinMode(RGB_PIN_GREEN, OUTPUT);
    pinMode(RGB_PIN_BLUE, OUTPUT);
    pinMode(BINARY_SWITCH_1, INPUT);
    pinMode(BINARY_SWITCH_2, INPUT);
    pinMode(BINARY_SWITCH_3, INPUT);
    pinMode(BINARY_SWITCH_4, INPUT);
    pinMode(MODE_SWITCH, INPUT);
    pinMode(ENTER_SWITCH, INPUT);
    pinMode(TRIG_PIN, OUTPUT);
    pinMode(ECHO_PIN, INPUT);
    dht.begin();
    Serial.begin(9600);
    Serial.println("Inicializando...");
}
```

### 3. Estruturas de Dados
A estrutura `Command` é definida para armazenar os comandos e suas representações binárias. Isso permite uma fácil associação entre o comando e a ação correspondente.

```cpp
struct Command {
    int bitValue;
    String name;
};
```

### 4. Funções de Ação
As funções de ação implementam operações específicas, como ligar/desligar LEDs, ler temperatura e distância. Cada função também fornece feedback através do monitor serial.

```cpp
void turnOffAll() {
    digitalWrite(LED_A, LOW);
    digitalWrite(LED_B, LOW);
    digitalWrite(LED_C, LOW);
    noTone(BUZZER);
    digitalWrite(RGB_PIN_RED, LOW);
    digitalWrite(RGB_PIN_GREEN, LOW);
    digitalWrite(RGB_PIN_BLUE, LOW);
    printIfChanged("[ACTION]: Todos foram desligados.");
}
```

### 5. Processamento de Comandos
O código processa comandos recebidos via Serial ou switches binários, executando a função correspondente.

```cpp
void processSerialCommand() {
    while (true) {
        if (Serial.available() > 0) {
            String command = Serial.readStringUntil('\n');
            if (command.length() > 0) {
                int bitSerialCommand = stringToCommand(command);
                if (bitSerialCommand != -1) {
                    executeCommand(bitSerialCommand);
                }
            }
        }
    }
}
```

## Tabela de Comandos e Funções

| Comando               | Função                        | Descrição                                          |
|----------------------|-------------------------------|---------------------------------------------------|
| `TURN_OFF_ALL`       | Desliga todos os dispositivos. | Desliga LEDs, buzzer e LEDs RGB.                  |
| `TURN_ON_LED_A`      | Liga o LED A.                 | Liga o LED conectado ao pino definido.            |
| `TURN_ON_LED_B`      | Liga o LED B.                 | Liga o LED conectado ao pino definido.            |
| `TURN_ON_LED_C`      | Liga o LED C.                 | Liga o LED conectado ao pino definido.            |
| `BUZZER_ON`          | Liga o buzzer.                | Ativa o buzzer para emitir som.                   |
| `BUZZER_OFF`         | Desliga o buzzer.             | Desativa o buzzer.                                |
| `TURN_ON_RED`        | Liga o LED vermelho do RGB.   | Ativa o LED vermelho do módulo RGB.               |
| `TURN_ON_GREEN`      | Liga o LED verde do RGB.      | Ativa o LED verde do módulo RGB.                  |
| `TURN_ON_BLUE`       | Liga o LED azul do RGB.       | Ativa o LED azul do módulo RGB.                   |
| `TURN_OFF_RED`       | Desliga o LED vermelho do RGB. | Desativa o LED vermelho do módulo RGB.            |
| `TURN_OFF_GREEN`     | Desliga o LED verde do RGB.   | Desativa o LED verde do módulo RGB.               |
| `TURN_OFF_BLUE`      | Desliga o LED azul do RGB.    | Desativa o LED azul do módulo RGB.                |
| `DIST_CHECK`         | Lê e imprime a distância medida. | Usa um sensor ultrassônico para medir distância.  |
| `TEMP_CHECK`         | Lê e imprime a temperatura.    | Usa o sensor DHT11 para medir a temperatura.      |

## Ideias Implementadas no Código

### 1. Controle de Dispositivos
O código permite o controle de LEDs e um buzzer através de comandos seriais e switches binários. Isso torna o projeto interativo e versátil, possibilitando várias formas de operação.

### 2. Monitoramento Ambiental
O projeto inclui um sensor de temperatura (DHT11) e um sensor de distância (ultrassônico). Isso permite que o sistema forneça informações sobre as condições ambientais, além de executar comandos.

### 3. Feedback Serial
Para cada ação realizada, uma mensagem é enviada ao monitor serial, fornecendo feedback sobre o que está acontecendo no sistema. Isso é útil para depuração e monitoramento.

### 4. Modularidade
As funções são bem definidas e organizadas, permitindo fácil manutenção e expansão do projeto. Novas funcionalidades podem ser adicionadas sem grandes modificações no código existente.

### 5. Interatividade
O uso de um switch de modo permite alternar entre diferentes modos de operação (comando serial e switches binários), oferecendo uma interface mais amigável e flexível para o usuário.

## Requisitos

- **Hardware**: Um microcontrolador Arduino, LEDs, buzzer, sensor DHT11, sensor ultrassônico, resistores, fios, etc.
- **Software**: IDE do Arduino instalada.

## Conclusão

Este projeto demonstra a aplicação de conceitos de controle de dispositivos e monitoramento ambiental usando Arduino. A estrutura modular e o feedback serial tornam o sistema fácil de usar e expandir, sendo uma ótima base para projetos futuros.
