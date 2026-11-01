# Heart Attack Detection

Classificação de ataques cardíacos com SVM a partir de dados clínicos de pacientes. Trabalho apresentado no UMC Summit (4ª edição), da Universidade de Mogi das Cruzes.

![Pôster do projeto](poster/poster_preview.jpg)

O pôster completo, em tamanho 80 x 100 cm, está na pasta `poster/`.

## O que a gente fez

Usamos uma base com 1.319 pacientes (idade, gênero, frequência cardíaca, pressão, glicose e dois exames do coração, CK-MB e troponina) e treinamos um modelo SVM para dizer se o paciente teve ou não um ataque cardíaco.

Tudo foi feito em Python, num notebook no Kaggle, com pandas, NumPy, Matplotlib, Seaborn e scikit-learn.

## Dados

A base é a "An Extensive Dataset for the Heart Disease Classification System", do Mendeley Data, com licença CC BY 4.0. O arquivo está em `data/Heart_Attack.csv` e as colunas estão explicadas em `data/README.md`.

## Como foi feito

Primeiro conferimos se havia valores nulos ou duplicados (não havia). Depois separamos 80% dos pacientes para treino (1.055) e 20% para teste (264), mantendo a proporção entre positivos e negativos.

Treinamos primeiro um SVM simples, com kernel RBF e C = 1. Em seguida usamos o GridSearchCV, com validação cruzada de 5 partes, para testar combinações de kernel, C e gamma. O melhor foi o kernel linear com C = 10. Os dados foram padronizados com StandardScaler dentro de um Pipeline.

## Resultados

O modelo simples acertou 75% no teste e o modelo ajustado chegou a 84%, ou seja, 221 dos 264 pacientes de teste. A média na validação cruzada foi 0,81 e a área sob a curva ROC (AUC) foi 0,91.

O modelo errou em 43 pacientes: 16 sem ataque que ele classificou como positivos e 27 com ataque que ele não detectou. Para os casos positivos, a precisão foi 0,89, o recall 0,83 e o F1 0,86.

CK-MB e troponina são as variáveis que mais pesam na decisão do modelo, e a idade vem em terceiro. Pressão, glicose e frequência cardíaca quase não mudam o resultado. Isso bate com a correlação: troponina 0,66, CK-MB 0,30 e idade 0,24.

![Correlação entre as variáveis](figures/03_correlacao.png)

![Matriz de confusão](figures/05_matriz_de_confusao.png)

Os outros gráficos estão na pasta `figures/` e no notebook.

## Observações

É um trabalho acadêmico, então o modelo não serve para diagnóstico. Como 27 pacientes com ataque não foram detectados, seria preciso testar em outra base antes de pensar em qualquer uso prático.

Também notamos 3 valores de frequência cardíaca acima de 150 (o máximo é 1111), que parecem erro na base. Deixamos eles como estavam para não mudar o resultado original.

## Como rodar

```bash
git clone https://github.com/devmatheusoliv/heart-attack-detection-svm.git
cd heart-attack-detection-svm
pip install -r requirements.txt
jupyter notebook notebooks/heart_attack_svm.ipynb
```

O notebook procura o CSV em `data/Heart_Attack.csv`. Se não achar, usa o caminho do dataset do Kaggle. No Colab, dá pra rodar `!git clone` com o mesmo endereço e abrir o notebook da pasta `notebooks/`.

## Autores

Matheus Luiz de Oliveira, João Pedro de Morais, Higor Barreto Campos e Fabiano Bezerra Menegídio, da Universidade de Mogi das Cruzes (Mogi das Cruzes, SP).

## Referência dos dados

Maghdid, S. S.; Rashid, T. A. (2022). An Extensive Dataset for the Heart Disease Classification System. Mendeley Data, V2. https://doi.org/10.17632/65gxgy2nmg.2

## Licença

Código sob licença MIT (arquivo `LICENSE`). Os dados continuam com a licença original, CC BY 4.0.
