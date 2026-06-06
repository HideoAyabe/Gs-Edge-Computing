# 🛰️ OrbitCore — Sonda de Exploração TRAPPIST-1

> Sistema embarcado de identificação, varredura e catalogação autônoma de exoplanetas, desenvolvido para a **Global Solution 2026 · Indústria Espacial** — disciplina **Edge Computing & Computer Systems** (FIAP).

🔗 **Simulação no Wokwi:** https://wokwi.com/projects/465213890222709761

---

## 📖 Descrição do Projeto

O **OrbitCore** simula o computador de bordo de uma sonda de exploração espacial operando no sistema **TRAPPIST-1** — um sistema estelar real, a cerca de 40 anos-luz da Terra, com sete exoplanetas conhecidos.

Conforme a sonda se aproxima de cada planeta (representado por uma **tag RFID**), o sistema:

1. Identifica o corpo celeste pelo seu UID.
2. Executa uma rotina de **varredura** com sinalização visual e sonora.
3. Registra a descoberta com **data e hora** (RTC).
4. Armazena os dados de forma **permanente** na EEPROM.
5. Exibe um **dossiê completo** do planeta (características físicas, orbitais e Índice de Similaridade com a Terra — ESI).

Toda a inteligência roda **localmente no microcontrolador**, sem depender de conexão contínua com a Terra — exatamente o cenário extremo de **Edge Computing em órbita**: recursos limitados, latência crítica e necessidade de autonomia.

---

## 🎯 Objetivo da Solução

Catalogar e priorizar exoplanetas diretamente na borda da rede (a própria sonda), resolvendo três problemas reais da exploração espacial:

- **Autonomia:** decisões e processamento sem retorno possível à estação base.
- **Persistência:** os dados sobrevivem a quedas de energia e reinicializações (EEPROM), evitando perda de missão.
- **Priorização:** o **ESI** indica quais planetas têm maior potencial de habitabilidade, orientando onde investir os próximos esforços de estudo.

**ODS relacionada:** ODS 9 — Indústria, Inovação e Infraestrutura.

---

## 🔧 Componentes Utilizados

| Componente | Função | Pinos (Arduino Uno) |
|---|---|---|
| **Arduino Uno** | Microcontrolador principal | — |
| **Leitor RFID MFRC522** | Identifica cada planeta pela tag | SS=10, RST=9, SCK=13, MOSI=11, MISO=12, 3.3V, GND |
| **Tags / Cartões RFID** | Representam os planetas (1b, 1c, 1d, 1e) | — |
| **Módulo RTC (DS3231 / DS1307)** | Data e hora reais do registro de missão | I²C → SDA=A4, SCL=A5, VCC=5V, GND |
| **LED RGB** | Status visual da operação | R=3, G=4, B=5 |
| **Buzzer** | Alertas sonoros | 2 |
| **EEPROM (interna do Uno)** | Armazenamento permanente dos dados | — |

**Bibliotecas:** `SPI`, `Wire`, `EEPROM` (nativas) · `MFRC522` · `RTClib`.

---

## ⚙️ Explicação do Funcionamento

### Inicialização (`setup`)
- Inicia comunicação serial (9600), SPI, leitor RFID e barramento I²C.
- Verifica o RTC; se houver perda de energia, ajusta a hora.
- Na primeira execução, formata a EEPROM usando um **valor mágico** de controle.
- Imprime a hora atual e o **registro de missão** já existente.

### Loop Principal
A sonda aguarda a aproximação de um planeta (uma tag RFID). Ao detectar:

- **Planeta reconhecido:**
  - Varredura → LED **amarelo** piscando + buzzer.
  - Sucesso → LED **verde** + tom contínuo.
  - Grava a descoberta na EEPROM (contador, ordem de descoberta, timestamp e ESI).
  - Exibe o dossiê completo e simula o envio dos dados para a **Central Prisma**.
- **Planeta não reconhecido:**
  - LED **vermelho** + sequência de tons de erro.

### Estados do LED RGB

| Cor | Significado |
|---|---|
| Apagado | Em espera |
| Amarelo (piscando) | Varredura em andamento |
| Verde | Identificação bem-sucedida |
| Vermelho | Falha no reconhecimento |

### Planetas catalogados

| Planeta | ESI | Destaque |
|---|---|---|
| TRAPPIST-1b | 0.30 | Quente, sem atmosfera |
| TRAPPIST-1c | 0.57 | CO₂ residual |
| TRAPPIST-1d | 0.71 | Atmosfera fina |
| TRAPPIST-1e | 0.85 | **Zona habitável** |

---

## 🔌 Estrutura do Circuito

**Leitor RFID MFRC522 (SPI)**
```
SDA/SS → 10      RST → 9
SCK    → 13      MOSI → 11
MISO   → 12      3.3V → 3.3V      GND → GND
```

**Módulo RTC (I²C)**
```
SDA → A4    SCL → A5    VCC → 5V    GND → GND
```

**LED RGB**
```
R → 3 (PWM)    G → 4    B → 5 (PWM)    Catodo comum → GND
```

**Buzzer**
```
(+) → 2        (-) → GND
```

> O diagrama completo das ligações pode ser visualizado diretamente na aba **diagram.json** do projeto no Wokwi.

---

## ▶️ Instruções de Execução

### Opção A — Simulador Wokwi (recomendado, sem instalação)
1. Acesse o link da simulação.
2. Clique no botão **▶ (play)** para iniciar.
3. Abra o **Serial Monitor** (9600 baud).
4. Clique em uma **tag RFID** no diagrama para simular a aproximação de um planeta.
5. Observe o LED, o buzzer e o dossiê impresso no monitor serial.
6. Teste uma tag não cadastrada para ver a rotina de falha.

### Opção B — Hardware físico (Arduino IDE)
1. Instale a **Arduino IDE**.
2. Em *Library Manager*, instale **MFRC522** e **RTClib**.
3. Abra o arquivo `sketch.ino`.
4. Selecione a placa **Arduino Uno** e a porta correta.
5. Monte o circuito conforme a tabela de ligações acima.
6. Faça o **upload** e abra o Serial Monitor a **9600 baud**.

---

## 🗂️ Estrutura do Repositório

```
.
├── sketch.ino       # Código-fonte (Arduino / C++)
├── diagram.json     # Esquema do circuito (Wokwi)
├── libraries.txt    # Bibliotecas utilizadas
└── README.md        # Este arquivo
```

---

## 👥 Integrantes do Grupo

> ⚠️ **Preencher antes da entrega.** Trabalhos sem o nome dos integrantes sofrem desconto na nota.

| Nome completo | RM |
|---|---|
| Hyan Hideo | 571005 |
| Elizeu Antonio | 569433 |
| Ian Tassiotto | 570812 |
| João Gabriel | 573685 |

---

<sub>FIAP · Global Solution 2026 · Indústria Espacial · Engenharia de Software — 1º Ano</sub>
