# TP1 — Baseline Clássico
## RSNA 2023 · Screening Mammography Breast Cancer Detection

**Disciplina:** Tópicos Especiais em Sistemas de Informação
**Grupo:** G6 — Marcos Felipe Arruda Lima, Adrian Lucas Pinheiro Silva, Igor Holanda Costa
**Desafio:** RSNA Screening Mammography Breast Cancer Detection (2023)
**Competição no Kaggle:** https://www.kaggle.com/competitions/rsna-breast-cancer-detection
**Link do repositorio github:** https://github.com/IgorHolanda-Kel/TP1-G6-mammography

## Sobre o projeto:

Baseline de pesquisa para o desafio de triagem de câncer de mama por mamografia, usando exclusivamente métodos clássicos de visão computacional (sem redes neurais profundas).

## Dados

Os dados são os da competição RSNA Screening Mammography Breast Cancer Detection, hospedados no Kaggle. Os rótulos do conjunto de teste oficial não são públicos, então todo o protocolo experimental foi construído sobre o conjunto de treino, com partição própria por paciente.

**Como obter os dados:**
1. Aceitar os termos da competição em
   https://www.kaggle.com/competitions/rsna-breast-cancer-detection/rules
2. Rodar o notebook diretamente no Kaggle (recomendado) — basta adicionar o
   dataset da competição ao notebook (`Add Input`), que ele fica disponível em
   `/kaggle/input/rsna-breast-cancer-detection/`.
3. Alternativamente, baixar via Kaggle API:
   ```bash
   kaggle competitions download -c rsna-breast-cancer-detection
   ```

Este trabalho usa uma amostra extraida e selecionada do conjunto de treino (critério e
semente descritos no notebook, seção de configuração), e não uma cópia
completa dos dados brutos — os mesmos não estão versionados neste repositório por conta de seu extenso tamanho cerca de 300GB.

## Estrutura do repositório

.
├── notebooks/
│   └── TP1_G6_mammography_baseline.ipynb
├── requirements.txt
└── README.md

## Como instalar as dependências

```git bash (Terminal VS code)
pip install -r requirements.txt
```

Se for rodar localmente (fora do Kaggle) e o `pydicom` não conseguir abrir
os arquivos DICOM comprimidos em JPEG Lossless, garanta que os pacotes
`pylibjpeg`, `pylibjpeg-libjpeg`, `pylibjpeg-openjpeg` e `python-gdcm` estão
instalados (já incluídos no `requirements.txt`).

## Como rodar o notebook do início ao fim

### Passo 1: Fundação e Geração dos Folds (Semanas 1 e 2)
1. Abra `notebooks/TP1_G6_mammography_baseline.ipynb` no Kaggle (ou em um
   ambiente Jupyter local com os dados baixados).
2. Ajuste, se necessário, a variável `DATA_DIR` na célula de configuração:
   - No Kaggle: `/kaggle/input/rsna-breast-cancer-detection`
   - Local: caminho onde os dados foram baixados.
3. Ajuste `OUTPUT_DIR` para uma pasta gravável (no Kaggle, `/kaggle/working`).
4. Execute todas as células em ordem (`Run All`) se desejar rode celular por celula(Metodo recomendado para encontrar mais facilmente possiveis erros). O notebook realiza, nesta
   ordem:
   - carregamento e EDA dos dados;
   - leitura de DICOM e extração de metadados;
   - pipeline de pré-processamento (remoção de fundo, recorte de ROI, CLAHE);
   - particionamento por paciente (`StratifiedGroupKFold`);
   - baseline trivial (classificador de classe majoritária) com métricas
     reportadas.
5. Os arquivos gerados (ex.: `train_with_folds.csv`) ficam salvos em
   `OUTPUT_DIR`.

### Passo 2: Extração de Características e Modelagem (Semana 3)
1. Abra o notebook `gp6-screening-mammography-semana-3.ipynb`.
2. **Dependência Crítica:** Ajuste a variável `FOLDS_CSV_PATH` no topo do código para apontar exatamente para o ficheiro `train_with_folds.csv` que acabou de gerar no Passo 1.
3. **Aviso de Execução (Timeout):** Devido ao limite de 12 horas de processamento contínuo no Kaggle, a variável de subamostragem `NEG_PER_POS` foi fixada em `2`. Recomendo manter a mesma para garantir a extração de radiomica e auxiliar na finalização dos modelos.
4. Ajuste o `DATA_DIR` e execute todas as células (`Run All`). 
5. O notebook irá extrair os descritores (GLCM, HOG, Radiomics), correr a grelha de modelos (GridSearch), fazer o estudo de ablação de pré-processamento e gerar os CSVs e gráficos finais de métricas (`results_week3.csv` e as matrizes de confusão).

## Reprodutibilidade

- Semente fixa (`SEED = 42`) usada em toda amostragem e particionamento.
- Nenhum caminho absoluto de máquina pessoal — apenas `DATA_DIR`/`OUTPUT_DIR`
  configuráveis no topo do notebook.
- Todas as dependências e versões declaradas em `requirements.txt`.

## Uso de IA generativa

Este projeto contou com apoio de IA generativa (Claude) para:
estruturação do pipeline de pré-processamento, revisão/depuração de código e para validação de que todos os campos requisitados estão presentes neste README. Todo o código e os resultados foram executados e verificados pelo grupo.