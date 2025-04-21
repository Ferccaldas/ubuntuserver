# 🔌 Sistema de Gestão de Energia – Ubuntu Server SCADA/IoT | IFSul

Este projeto tem como objetivo desenvolver um **Sistema de Gestão de Energia de baixo custo**, baseado em tecnologias livres e modulares, com foco em monitoramento, análise e controle de sistemas fotovoltaicos conectados à rede elétrica com armazenamento em baterias. O sistema foi implementado no ambiente do Instituto Federal Sul-rio-grandense (IFSul) e será utilizado como base para o Trabalho de Conclusão de Curso do curso de Engenharia Elétrica.

---

## 🎯 Objetivo do Projeto

O sistema proposto visa coletar dados em tempo real sobre **geração, consumo e armazenamento de energia** a partir de inversores fotovoltaicos híbridos instalados no IFSul, mais especificamente o **Huawei SUN2000-5KTL-L1** e um inversor **Goodwe**. A partir desses dados, será possível:

- Monitorar o desempenho energético do sistema fotovoltaico;
- Armazenar os dados em um banco de séries temporais;
- Exibir gráficos e relatórios em tempo real;
- Criar rotinas inteligentes de controle de energia (ex: exportar energia em horários de pico tarifário);
- Integrar a inteligência artificial no sistema para **interações inteligentes** com os dados.

---

## 🧱 Arquitetura do Sistema

A arquitetura do sistema é dividida em quatro camadas:

### 🔌 Camada 1 – Aquisição de Dados
- ESP32 com conversor RS485 realiza a leitura via **protocolo MODBUS RTU** dos dados dos inversores Huawei e Goodwe.
- Registradores lidos incluem: potência ativa, energia acumulada, tensão, corrente, temperatura, estado da bateria (SOC) e status do inversor.

### 🌐 Camada 2 – Comunicação
- O ESP32 envia os dados via Wi-Fi para um **servidor local Ubuntu 22.04 LTS**.
- O servidor utiliza **Node-RED** como middleware para recebimento dos dados via **HTTP POST** ou **MQTT**.

### 🧠 Camada 3 – Processamento e Armazenamento
- Os dados são processados no Node-RED e enviados ao **InfluxDB**, banco de dados de séries temporais.
- Um script automático realiza **backups diários** dos dados do InfluxDB.

### 📊 Camada 4 – Visualização e Controle
- O **Grafana** é utilizado para exibir dashboards com métricas como geração diária, consumo, SOC da bateria e energia exportada.
- Regras de controle podem ser configuradas no Node-RED, como:
  - Exportar energia para a rede apenas durante o horário de ponta;
  - Priorizar o carregamento das baterias com base no SOC e na geração instantânea.

---

## 💻 Tecnologias Utilizadas

- 🐧 **Ubuntu Server 22.04 LTS**
- 🔧 **Node-RED** (lógica de fluxo)
- 📊 **Grafana** (dashboards em tempo real)
- 📂 **InfluxDB** (armazenamento de séries temporais)
- 📡 **Mosquitto MQTT** (protocolo leve de comunicação)
- 🔐 **Autenticação via bcrypt** no Node-RED
- 💾 **Backup automatizado via cron**
- ⚡ **Protocolo MODBUS RTU** (comunicação com inversores)
- 📶 **ESP32 com RS485 TTL**

---

## 🔍 Resultados Esperados

Ao final do projeto, será possível:
- Monitorar remotamente os dados de geração e armazenamento de energia do sistema fotovoltaico do IFSul;
- Reduzir custos com energia elétrica através de estratégias inteligentes de exportação e carregamento;
- Tornar o sistema extensível para qualquer outro cenário com inversores compatíveis com MODBUS RTU;
- Interagir com a base de dados através de **perguntas em linguagem natural** utilizando IA no Node-RED, como:  
  “Qual foi a energia total gerada nesta semana?”  
  “Qual o horário de maior exportação ontem?”  
  “Como está o nível de carga das baterias?”

---

## 📦 Organização do Repositório

| Arquivo/Dir                | Descrição |
|---------------------------|-----------|
| `completesetup`           | Script automatizado para configurar todo o servidor |
| `fluxos/`                 | Exportações dos fluxos do Node-RED |
| `README.md`               | Documento de descrição do projeto |
| `LICENSE`                 | Licença de uso do código (MIT recomendada) |

---

## 🛠️ Como Usar

1. Faça o clone do repositório:
   ```bash
   git clone https://github.com/Ferccaldas/ubuntuserver.git
   cd ubuntuserver
   chmod +x completesetup
   ./completesetup
