# Categorização de Bens de Candidatos com Deep Learning (2006–2022)
10 de março de 2025

## Geral
---

Este projeto tem como objetivo a padronização e categorização automatizada da base de bens declarados por candidatos(as) em eleições brasileiras entre 2006 e 2022, disponibilizada pelo TSE. A base original apresenta alto grau de inconsistência, com mais de 4 milhões de registros e uma grande variedade de descrições livres e categorias não padronizadas ao longo dos anos.

A primeira etapa envolveu a **reclassificação manual** de 54 categorias originais para **seis categorias principais**, representando classes gerais de patrimônio: imóveis, veículos, investimentos, participações societárias, outros bens e ausência de bens declarados.

Com essa categorização consolidada, a segunda etapa consistiu no **treinamento de um modelo de rede neural baseado no BERTimbau**, um modelo BERT pré-treinado para a língua portuguesa. A rede passou por *fine-tuning* supervisionado para aprender a classificar automaticamente as descrições livres dos bens (campo `DS_BEM_CANDIDATO`) nas novas categorias padronizadas.

O modelo demonstrou **altíssima performance**, atingindo uma taxa de acerto de 99,3% em uma validação manual amostral e 95,3% quando comparado diretamente às classificações manuais.

O projeto combina técnicas de pré-processamento em R e modelagem com NLP em Python, sendo um exemplo robusto de aplicação prática de **redes neurais em PLN (Processamento de Linguagem Natural)** para resolver um problema real de padronização em grandes bases públicas.

## Escopo

A base de dados de bens de candidatos do TSE possui entre outras as seguintes variáveis:
- DS_BEM_CANDIDATO:  Descrição detalhada do bem material patrimonial da candidata ou candidato.
- DS_TIPO_BEM_CANDIDATO: Tipo do bem patrimonial material declarado pela candidata ou candidato.

No entanto, os dados apresentam os seguintes problemas:
- Existem 54 categorias diferentes em DS_TIPO_BEM_CANDIDATO.
- Não há padronização das categorias entre as eleições.
- Não existem dados categorizados para os anos de 2006 e 2008.
- DS_BEM_CANDIDATO é um campo autodeclarado, sem a existência de padronização
- Há um total de mais 4 milhões de observações na base toda.

## Objetivo

- Categorizar os dados em DS_BEM_CANDIDATO a partir de um conjunto menor  e mais preciso de categorias com o uso de rede neural;
## Tarefa 1: Classificação Manual

Reduzimos as 52 categorias únicas de DS_TIPO_BEM_CANDIDATO para todas as eleições entre 2006 e 2022 em 6 categorias principais:

- Imóveis e Propriedade
- Investimentos Financeiros
- Outros Bens e Direitos
- Participações Societárias e Créditos
- Veículos
- Nenhum bem a declarar

Esta recategorização foi feita de forma manual e ad hoc. No link abaixo encontra-se a planilha com o match entre as 52 categorias originais e as 6 categorias sugeridas:

https://docs.google.com/spreadsheets/d/1wHH4bp9nV9YcfwpUAGbruavY4E5ddYd4m6QgDySVYEI/edit?usp=sharing

## Tarefa 2: Categorização da base com Rede Neural

Após a recategorização da base entre as seis categorias sugeridas na Tarefa 1, treinamos um modelo de **rede neural** com **aprendizado supervisionado** para classificar os dados da variável **DS_BEM_CANDIDATO**.

Utilizamos o modelo BERTimbau, um modelo BERT pré-treinado em língua portuguesa, contendo 24 camadas e 335 milhões de parâmetros. O modelo passou por fine-tuning utilizando dados supervisionados da base de bens de candidatos, com as seis categorias sugeridas na Tarefa 1 servindo como rótulos para o treinamento supervisionado.

Link do modelo: https://huggingface.co/neuralmind/bert-base-portuguese-cased

## Resultados

A classificação dos dados atingiu um alto índice de acurácia. Em uma validação manual com uma amostra de 900 observações (100 amostras para cada eleição entre 2006 a 2022), tivemos uma taxa de acerto de 99,3%. 

Quando comparamos o resultado obtido com o modelo BERT com os labels, a acurácia cai para 95,3%. Isso se deve ao fato de que existem erros na própria classificação do TSE e por algumas classificações confusas entre o que é considerado "Investimento Financeiro" e "Participações Societárias e Créditos". 

Os resultados da análise na amostra podem ser encontrados em: 

https://docs.google.com/spreadsheets/d/1A5ryn0lpfv_wI_2uSFN8WWpuabEPbbBzi7LkEYjcMX4/edit?usp=sharing

## Arquivos

- **output/bd02_2006_2022_final_bert_class.parquet** - base final com as classificações preditas pela rede neural BERT para todas as eleições;
- **modelo\mod02_bert_final** - Modelo BERTimbau com os parâmetros finais;
- **BERT - Categorizacao.ipynb** - arquivo jupyter com os códigos para aplicação do modelo BERTimbau para categorização da coluna;
- **BERT - Treino Colab.ipynb** - arquivo jupyter com os códigos de treinamento no Google Colab do modelo BERTimbau;
- **BERT - Treino.ipynb** - arquivo jupyter com os códigos de treinamento fora do Google Colab do modelo BERTimbau;
- **scriptA30_ML_Bens.R** - script com a preparação e limpeza dos dados para treino da rede neural. Aplicação também uma modelo de classificação com regressão logística multinomial que não foi usada no arquivo final. Salvamento do arquivo final; **output/bd02_2006_2022_final_bert_class.parquet** com a classificação BERTimbau.
- **classificacao_manual/primeira_categorizacao.xlsx** - planilha com a reclassificação das 54 categorias do TSE;
- **validacao_manual/amostra_bens_candidatos.xlsx** - planilha com a validação manual de uma amostra com 900 observações aleatórias do arquivo **output/bd02_2006_2022_final_bert_class.parquet**;
- **documentos/NLP com Deep Learning.pptx** - apresentação sobre Deep Learning, Transformers e o estado da arte em tarefas NLP.







