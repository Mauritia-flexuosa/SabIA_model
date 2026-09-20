# 🦜 SabIA: Classificação Acústica de Aves Brasileiras com Inteligência Artificial

![Status](https://img.shields.io/badge/Status-Concluído-success)
![Acurácia](https://img.shields.io/badge/Acurácia-91%25-blue)
![Espécies](https://img.shields.io/badge/Espécies-216-orange)
![Licença](https://img.shields.io/badge/Licença-CC%20BY--NC%204.0-lightgrey)

O [SabIA](https://mauritia-flexuosa.github.io/SabIApp/) é um motor de Inteligência Artificial otimizado para dispositivos móveis (Edge AI) focado na identificação offline de aves brasileiras através do seu canto. 

Este repositório contém o pipeline completo de *Machine Learning* utilizado para treinar o modelo que alimenta o aplicativo comercial SabIA (em breve estará disponível para testes no Android). O projeto resolve o desafio da bioacústica transformando matrizes de áudio do mundo real em espectrogramas visuais e processando-os através de uma rede neural convolucional de alta eficiência.

---

## 🎯 Resultados e Performance

* **Cobertura:** 216 espécies nativas do Brasil.
* **Volume de Dados:** 43.200 espectrogramas balanceados (200 amostras válidas por espécie).
* **Acurácia em Validação Inédita:** ~91% (Lidando com ruídos reais de vento, sobreposição acústica e variações de distância).
* **Tamanho do Modelo Final (TFLite):** Otimizado para rodar 100% offline em aparelhos Android com uso mínimo de CPU/Bateria.

---

## 🏗️ Arquitetura e Engenharia de Dados

Este repositório está dividido em duas frentes principais de engenharia:

### 1. Captação, Pré-processamento e Compliance (`aquisição_preprocessamento_audio.ipynb`)
Script responsável pela mineração, padronização e auditoria legal dos dados brutos:
* **Integração Segura (API v3):** Busca automatizada no Xeno-canto utilizando a API v3 com chamadas autenticadas de forma invisível.
* **Compliance de Direitos Autorais:** Filtro rigoroso na nuvem que descarta automaticamente gravações com licença restritiva de obras derivadas (`CC ND - No Derivatives`), blindando o dataset juridicamente para distribuição acadêmica e publicação no Kaggle.
* **Rastreabilidade e Metadados:** Geração simultânea de um arquivo estruturado `metadata.csv` (contendo geolocalização, data, horário, autor e tipo de canto da ave). Essa camada de dados tabulares é ideal para conectar a ferramentas de visualização e compor dashboards interativos com a distribuição espacial das espécies.
* **Conversão Visual:** Transforma o sinal de áudio em Espectrogramas de Mel utilizando a paleta de cores *Viridis* (ideal para destacar assinaturas acústicas).
* **Tolerância a Falhas (Resume):** Mecanismo inteligente que identifica a última etapa salva em caso de queda de conexão, retomando o processamento sem corromper dados ou gerar duplicatas.

### 2. Treinamento da Rede Neural (`preparacao_e_modelo_efficient_v2B1.ipynb`)
O modelo foi construído utilizando **Transfer Learning** sobre a arquitetura `EfficientNetV2B1` com as seguintes técnicas avançadas:
* **Data Augmentation Seguro:** Injeção de ruído e simulação espacial (`RandomBrightness`, `RandomTranslation`, `RandomZoom` horizontais) sem distorcer as frequências em Hertz.
* **Treinamento em 2 Fases (Fine-tuning):** 
  1. *Aquecimento (Warm-up):* Base congelada para treinar apenas o topo denso e proteger os pesos pré-treinados.
  2. *Descongelamento Profundo:* Ajuste fino com taxa de aprendizado reduzida (`1e-4`) e callbacks de `EarlyStopping` e `ReduceLROnPlateau`.

