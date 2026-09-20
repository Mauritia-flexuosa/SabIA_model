    # 🦜 SabIA: Classificação Acústica de Aves Brasileiras com Inteligência Artificial

![Status](https://img.shields.io/badge/Status-Concluído-success)
![Acurácia](https://img.shields.io/badge/Acurácia-91%25-blue)
![Espécies](https://img.shields.io/badge/Espécies-172-orange)
![Licença](https://img.shields.io/badge/Licença-CC%20BY--NC%204.0-lightgrey)

O **SabIA** é um motor de Inteligência Artificial otimizado para dispositivos móveis (Edge AI) focado na identificação offline de aves brasileiras através do seu canto. 

Este repositório contém o pipeline completo de *Machine Learning* utilizado para treinar o modelo que alimenta o [Aplicativo SabIA](https://mauritia-flexuosa.github.io/SabIApp/) (em breve estará disponível para testes no Android). O projeto resolve o desafio da bioacústica transformando matrizes de áudio do mundo real em espectrogramas visuais e processando-os através de uma rede neural convolucional de alta eficiência.

---

## 🎯 Resultados e Performance

* **Cobertura:** 172 espécies nativas do Brasil (Selecionadas após rigorosa auditoria legal de licenças restritivas).
* **Volume de Dados:** 43.200 espectrogramas balanceados (200 amostras válidas por espécie).
* **Acurácia em Validação Inédita:** ~91% (Lidando com ruídos reais de vento, sobreposição acústica e variações de distância).
* **Modelos Disponibilizados:**
  * **`.tflite`**: Tamanho de modelo final ultraleve, otimizado para rodar 100% offline em aparelhos Android com uso mínimo de CPU/Bateria.
  * **`.keras`**: Pesos e arquitetura originais preservados. Ideal para cientistas de dados e biólogos que desejam realizar análises estruturais, auditorias ou *Fine-Tuning* (Transfer Learning).

---

## 🏗️ Arquitetura e Engenharia de Dados

Este repositório está dividido em duas frentes principais de engenharia:

### 1. Captação, Pré-processamento e Compliance (`aquisição_preprocessamento_audio.ipynb`)
Script responsável pela mineração, padronização e auditoria legal dos dados brutos:
* **Integração Segura (API v3):** Busca automatizada no Xeno-canto utilizando a API v3 com chamadas autenticadas de forma invisível.
* **Compliance de Direitos Autorais:** Filtro rigoroso na nuvem que descarta automaticamente gravações com licença restritiva de obras derivadas (`CC ND - No Derivatives`), blindando o dataset juridicamente para distribuição acadêmica e publicação no Kaggle.
* **Transparência, Auditoria e Metadados (`dataset_sabia_metadados.csv`):** Inclusão de um arquivo estruturado atuando como tabela-mestre de auditoria. Ele rastreia a origem exata de cada amostra, contendo ID original, geolocalização (latitude/longitude), data, horário, autor e tipo de canto da ave. Essa transparência garante a validação completa do modelo e permite conectar os dados a ferramentas de visualização espacial.
* **Conversão Visual:** Transforma o sinal de áudio em Espectrogramas de Mel utilizando a paleta de cores *Viridis* (ideal para destacar assinaturas acústicas).
* **Tolerância a Falhas (Resume):** Mecanismo inteligente que identifica a última etapa salva em caso de queda de conexão, retomando o processamento sem corromper dados ou gerar duplicatas.

### 2. Treinamento da Rede Neural (`preparacao_e_modelo_efficient_v2B1.ipynb`)
O modelo foi construído utilizando **Transfer Learning** sobre a arquitetura `EfficientNetV2B1` com as seguintes técnicas avançadas:
* **Data Augmentation Seguro:** Injeção de ruído e simulação espacial (`RandomBrightness`, `RandomTranslation`, `RandomZoom` horizontais) sem distorcer as frequências em Hertz.
* **Treinamento em 2 Fases (Fine-tuning):** 
  1. *Aquecimento (Warm-up):* Base congelada para treinar apenas o topo denso e proteger os pesos pré-treinados.
  2. *Descongelamento Profundo:* Ajuste fino com taxa de aprendizado reduzida (`1e-4`) e callbacks de `EarlyStopping` e `ReduceLROnPlateau`.

---

## ⚖️ Licença de Uso (Importante)

A preservação da biodiversidade é um esforço coletivo. Por isso, os scripts deste repositório e o Dataset atrelado estão sob a licença **Creative Commons Atribuição-NãoComercial 4.0 Internacional (CC BY-NC 4.0)**.

Você é livre para:
* **Compartilhar:** Copiar e redistribuir o material em qualquer suporte ou formato.
* **Adaptar:** Remixar, transformar e criar a partir do material (como para projetos de faculdade ou pesquisas científicas).

Sob as seguintes condições:
* **Atribuição:** Você deve dar o crédito apropriado a este repositório e seu autor e fornecer um link para esta licença.
* **Uso Não-Comercial:** Você **NÃO PODE** utilizar o modelo treinado, os scripts ou o dataset deste repositório para finalidades comerciais (incluindo, mas não se limitando a: empacotar a IA em aplicativos pagos, aplicativos com anúncios, ou serviços de consultoria comercial).

Para dúvidas, pesquisas acadêmicas ou parcerias, sinta-se à vontade para abrir uma `Issue` ou entrar em contato!
