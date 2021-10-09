# Classificação de interferências em bancos de dados de ferramentas CAE 3D via Métodos de Aprendizado Supervisionado

#### Aluno: [Tiago Veroneze](https://github.com/tiagovero)
#### Orientadora: [Manoela Kohler](https://github.com/manoelakohler)
#### Co-orientador: [José Bermudez]

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- [Link para o código](https://github.com/tiagovero/BI_Master_Project/)

---

### Resumo

Durante a realização de um projeto de uma plataforma de petróleo são identificados pelas ferramentas de modelagem 3D dezenas ou centenas de milhares de interferências entre objetos que precisam ser analisadas pelos projetistas. Com o intuito de minimizar o hh envolvido nesta tarefa, criou-se um modelo utilizando métodos de aprendizado supervisionado para a classificação destas interferências como graves ou não. Utilizou-se dados de um projeto já realizado para criação do dataset e foram testados vários classificadores obtendo o melhor resultado de 88.38% com Random Forest, o que nos permite fazer uma priorização no tratamento destes problemas, tendo um ganho razoável de hh.

### 1. Introdução

As ferramentas CAE  (Computer Aided Engineering) baseadas em bancos de dados são utilizadas para a realização de projetos de plataformas de petróleo.
Umas dessas ferramentas contém a modelagem gráfica em 3D de todos os elementos do projeto. Essa ferramenta identifica automaticamente as interferências físicas entre diferentes objetos do projeto e os registra em seu banco de dados SQL.
Entretanto, são identificados, usualmente, milhares de clashes em cada projeto, ficando muito trabalhoso e praticamente inviável para os projetistas analisarem manualmente cada um, para identificar quais realmente precisam ser tratados.
A ideia deste trabalho foi utilizar machine learning para identificar clashes graves do software Smart3D, utilizado para modelagem 3D de um FPSO  (plataforma do tipo Floating Production Storage and Offloading), baseado em atributos das interferências e dos objetos envolvidos retirados desses bancos de dados. 

### 2. Modelagem

Realizou-se a extração via SQL dos bancos de dados de projeto de uma determinada plataforma de petróleo em 5 momentos distintos da sua realização, identificando as interferências em cada momento e trazendo o máximo de informações de cada interferência e dos objetos envolvidos.
As interferências que foram tratadas em algum momento pelos projetistas foram classificadas como graves e aquelas que foram ignoradas ou aprovadas foram classificadas como não graves, criando o rótulo desses itens.
A partir disso, criou-se um modelo em python para classificar as interferências de futuros projetos.
O pré-processamento incluiu: Retirar Coluna OID, Tratar Missing Data, Balanceamento (oversampling), transformação de inputs categóricos em numéricos (dummy coding), normalização dos dados e separação das bases de treino e teste. Foi feita, também, uma seleção de atributos que se demonstrou desnecessária e prejudicial aos resultados, então, foi retirada do modelo final.
O modelo foi treinado utilizando-se Random Forest, Decision Tree, SVM e XGBoost.
Foram criados loops no código variando-se os parâmetros de treino dos classificadores até encontrar os parâmetros otimizados para cada um. Após isso, os loops foram retirados, com o intuito de manter o código rodando num tempo razoável, e foram mantidos os parâmetros otimizados de cada um.

### 3. Resultados

Para avaliar o resultado de cada classificador, apresentados na Tabela 1, além da acurácia, foi criado um outro índice para avaliar os falsos negativos, que são críticos neste estudo.

		Tabela 1 – Resultados dos classificadores na base de teste
	Modelo			Acurácia	Acurácia contra falsos negativos	Média
	Random Forest	        87.30%		89.45%				        88.38%
	Decision Tree	        86.30%		85.30%				        85.80%
	XGBoost			87.10%		84.06%					85.58%
	SVM			83.90%		87.21%					85.55%

Entende-se que esses resultados são limitados pela qualidade dos dados utilizados, uma vez que podem existir interferências graves no projeto que não foram tratadas pelos projetistas (sendo, então, classificadas no nosso insumo como não graves) ou interferências não graves que deixaram de existir devido à uma modificação de um de seus objetos (sendo classificadas como graves).
O melhor classificador encontrado foi Random Forest, com uma média de 88.38%.


### 4. Conclusões

Este resultado não permite que se trate nos futuros projetos apenas as interferências classificadas pelo modelo como graves, porém, permite que seja priorizado o tratamento destas, o que pode trazer um ganho considerável de hh, visto que são dezenas ou centenas de milhares de interferências encontradas em cada projeto e apenas uma parcela das mesmas (cerca de 20%) são graves.

---

Matrícula: 192.671.005

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
