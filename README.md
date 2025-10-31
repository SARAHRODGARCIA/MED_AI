# 🏥 MED.AI – Detecção de Fraudes em Planos de Saúde com Machine Learning

**Aluna:** Sarah Rodrigues Garcia  
**Curso:** Ciências de Dados – EBAC  

Estrutura do Projeto:
A entrega do projeto foi organizada em três partes complementares, que juntas apresentam tanto a
fundamentação técnica quanto a comunicação estratégica dos resultados:
1. Documento Técnico-Descritivo:
Este documento apresenta uma visão geral das principais etapas do projeto: Coleta de Dados,
Modelagem e Conclusões; Onde se é explicada a criação e integração dos bancos de dados,
estruturação do Data Lake, geração de dados sintéticos, seleção e parametrização dos
modelos de Machine Learning, além de outros aspectos relevantes da solução desenvolvida.
2. Pipeline Técnica Detalhada (Markdown – GitHub):
Contém toda a pipeline de desenvolvimento do modelo de Machine Learning, documentada em
markdowns que descrevem, passo a passo, o processo de Coleta de Dados, Modelagem e
Conclusões.
3. Vídeo Narrado (Apresentação Executiva):
Vídeo explicativo baseado em um PowerPoint narrado por mim, voltado para stakeholders e
tomadores de decisão, com linguagem acessível e foco em demonstrar o funcionamento do
modelo, suas etapas principais e o impacto da solução na identificação de fraudes.
Referências de Entrega:
🔗 Link da Pipeline (GitHub): https://github.com/SARAHRODGARCIA/MED_AI/edit/main/README.md
🎥 Link do Vídeo Narrativo: https://youtu.be/RfbtyROXCWc?si=C9F34JRPt5Zno4X2
---

## 1. Coleta de Dados

O projeto visa identificar fraudes em transações médicas, distribuídas em dois sistemas distintos:  

1. **Banco de Dados Financeiro do Plano de Saúde:** informações de pagamentos, valores, datas, prestadores, métodos de repasse, etc.  
2. **Banco de Dados Operacional dos Prestadores de Saúde:** dados clínicos, autorizações, códigos de procedimentos, beneficiários e prestadores.  

### Integração dos Bancos

- **Chaves de União:** `id_transacao`, `id_prestador`, `id_beneficiario`, `data_autorizacao` ou `data_pagamento`.  
- **Data Lake / Data Warehouse:** camada intermediária para integrar, limpar e padronizar os dados.  
- **Processamento em Tempo Real:** usando tecnologias como Apache Kafka e Spark Streaming, as transações são integradas e enviadas ao modelo ML em segundos.  

### Banco de Dados Sintético

Devido à LGPD e à falta de dados financeiros públicos, foi criado um banco sintético:  

- **Arquivo CSV:** `banco_dados_sintetico_operadora.csv` – este arquivo deve ser usado como base do projeto  
- Total de registros: 10.000  
- 90% registros regulares, 10% fraudes  
  - Phantom Billing: 3%  
  - Upcoding: 3%  
  - Duplicidade de Pagamento: 3%  

#### Criação das Fraudes

- **Phantom Billing:** `valor_pago > 0` e `data_realizacao = NaT`  
- **Upcoding:** `status_saude = -1` + `procedimentos_urgencia = 'sim'`  
- **Duplicidade de Pagamento:** valores pagos muito altos  

Todos os dados foram tratados, padronizados e normalizados, garantindo coerência relacional.

---

## 2. Modelagem

### Divisão de Dados

- Treino: 70%  
- Teste: 30%  
- **Balanceamento:** SMOTE aplicado na base de treino para lidar com desbalanceamento das classes minoritárias.  

### Algoritmos

- **XGBoost**  
- **LightGBM**  

Foram realizados quatro experimentos:  

| Modelo      | Base de Treino            | Observação                  |
|------------|--------------------------|----------------------------|
| XGBoost    | Não balanceada           | Base bruta normalizada     |
| XGBoost    | Balanceada               | SMOTE aplicado             |
| LightGBM   | Não balanceada           | Base bruta normalizada     |
| LightGBM   | Balanceada               | SMOTE aplicado             |

### Treinamento e Otimização

- Parâmetros otimizados via **RandomSearch**  
- Avaliação em **base de teste 30%**  
- **Dados utilizados:** `banco_dados_sintetico_operadora.csv`  

```python
import pandas as pd

# Carregar CSV
df = pd.read_csv('banco_dados_sintetico_operadora.csv')

# Exemplo de visualização
df.head()

***Conclusões***

XGBoost apresentou melhor desempenho geral, mesmo sem balanceamento.

Modelo eficiente na distinção entre registros regulares e fraudulentos.

SHAP foi utilizado para interpretação das variáveis mais relevantes, garantindo transparência e auditabilidade.

O projeto demonstrou a viabilidade de detecção de fraudes médicas usando ML explicável e eficiente, servindo como base para implementações futuras em ambientes reais.
