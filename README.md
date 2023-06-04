# Projeto de BI para empresa Hermex Log no Qlik Sense Desktop

Projeto parte do Alura Challenge BI 2ª Semana

| :placard: Vitrine.Dev |     |
| -------------  | --- |
| :sparkles: Nome        | **Hermex Log Qlik Sense**
| :label: Tecnologias | Qlik Sense Desktop
| :rocket: URL         | https://trello.com/b/570mANgV/challenge-bi-semana-2
| :fire: Desafio     | [https://url-do-desafio.com.br](https://www.alura.com.br/challenges/bi-3/semana-02-logistica-dados?utm_source=ActiveCampaign&utm_medium=email&utm_content=%5BChallenge+BI%5D+Segunda+semana+liberada+%F0%9F%94%93&utm_campaign=%5BCHALLANGE%5D+%28BI+-+3%C2%AA+ed+%29+E-mail+avisando+libera%C3%A7%C3%A3o+da+aula+02++%2B+convite+live+github+e+vitrine+dev&vgo_ee=7CTKyjQnWA4jiE3yBlF7q9A6ssAik5JEHf9FGO%2FXPuIpjBI%3D%3AEaQRLfaQ9mjGI1iP%2FQbfzdyEh0tDyPop)

<!-- Inserir imagem com a #vitrinedev ao final do link -->
![image](https://github.com/PedroMoeziaJr/Hermex-Log-Projeto-de-BI/assets/112977342/cbf4dadd-8ef8-4761-b838-a4efc8545ec2)#vitrinedev



## Detalhes do projeto da Semana 2

Detalhes dos requisitos do projeto:

|ETL no Qlik Desktop|

Depois de extrair os dados da organização hermex, fiz a transformação dos dados da seguinte forma:

Atribui nome as tabelas e padronizei o nome da coluna id_produto das tabelas para que o Qlik Sense pudesse fazer o relacionamento das tabelas e acabar com o problema de chaves sintéticas.

Além disso renomeei o nome do campo Qunatidade da tabelas "estoque" para "quantidade estoque" para não haver o relacionamento com a coluna "quantidade" de outra tabela.

Outro detalhe é que o campo UF tinha os registro dos estados da seguinte forma: "BR-PA", sendo que interessa para o registro apenas o PA.

Desta forma utilizei a função replace do Qlik Sense no carregamento de dados para o registro apresentar somente o as siglas da UF.

    Replace(UF, 'BR-', '') as UF

O campo ESTADO estava sem registro, diante disso criei uma condicional no Qlik Sense na parte de carregamento de dados para poder poder preencher os registros dos ESTADOS.

    IF(UF = 'BR-PA' or UF = 'Pará','') 

E assim por diante em todos os UF registrados 

No campo UF com o registro DF relacionado com Distrito Federal, coloquei a palavra Brasília para o gráfico de mapas do Qlik Sense identificar o DF no Brasil.

O campo da "Data da compra" tem também com registro a hora da compra. Por isso usei a função DATE para extrair somente a data do campo.

    Time("Data da compra") as [Hora da Compra]

e a criei outro campo para extrair apenas a hora da compra.

    Time("Data da compra") as [Hora da Compra]

-As mesmas correções fazer no campo Data Previsão e data de entrega.

    Date("Data previsão") as [Data Prevista],
    Time("Data previsão") as [Hora Prevista],

E ASSIM FICOU O SCRIPT DO QLIK SENSE:

	Produtos:
	LOAD
	    id_produto,
	    categoria_produto,
	    preço
	FROM [lib://Base de Dados Empresa Hermex Log/base-de-dados-Hermex.xlsx]
	(ooxml, embedded labels, table is Produtos);

	Estoque:
	LOAD
	    "ID Produto" as id_produto,
	    "Data atualização",
	    Quantidade as [Quantidade Estoque]
	FROM [lib://Base de Dados Empresa Hermex Log/base-de-dados-Hermex.xlsx]
	(ooxml, embedded labels, table is Estoque);

	Veículos:
	LOAD
	    "ID veículos" as [ID Veículo],
	    Tipo,
	    Status
	FROM [lib://Base de Dados Empresa Hermex Log/base-de-dados-Hermex.xlsx]
	(ooxml, embedded labels, table is Veículos);

	Pedidos:
	LOAD
	    "ID Pedido",
	    "ID Produto" as id_produto,
	    Quantidade,
	    "ID Veículo",
	    "Status do pedido",

	    /*"Data da compra",*/

	    Date("Data da compra") as [Data da Compra],
	    Time("Data da compra") as [Hora da Compra],

	    /*"Data de entrega"*/

	    Date("Data de entrega") as [Data de Entrega],
	    Time("Data de entrega") as [Hora de Entrega],


	    /*"Data previsão",*/

	    Date("Data previsão") as [Data Prevista],
	    Time("Data previsão") as [Hora Prevista],


	    Replace(UF, 'BR-', '') as UF,
		IF(UF = 'BR-SP' or UF = 'São Paulo', 'São Paulo',
	       	IF(UF = 'BR-PA' or UF = 'Pará', 'Pará',
		IF(UF = 'BR-PI' or UF = 'Piauí', 'Piauí',
		IF(UF = 'BR-MG' or UF = 'Minas Gerais', 'Minas Gerais', 
		IF(UF = 'BR-ES', 'Espírito Santo',
		IF(UF = 'BR-RJ', 'Rio de Janeiro',
		IF(UF = 'BR-RN', 'Rio Grande do Norte',
		IF(UF = 'BR-MS', 'Mato Grosso do Sul',
		IF(UF = 'BR-DF', 'Brasília',
		IF(UF = 'BR-AP', 'Amapá',
		IF(UF = 'BR-CE', 'Ceará',
		IF(UF = 'BR-PR', 'Paraná',
		IF(UF = 'BR-RR', 'Roraima',
		IF(UF = 'BR-TO', 'Tocantins',
		IF(UF = 'BR-SC', 'Santa Catarina',
		IF(UF = 'BR-AM', 'Amazonas',
		IF(UF = 'BR-PE', 'Pernambuco',
		IF(UF = 'BR-RS','Rio Grande do Sul',
		IF(UF = 'BR-RO', 'Rondônia',
		IF(UF= 'BR-MT', 'Mato Grosso',
		IF(UF='BR-MA', 'Maranhão',
		IF(UF='BR-AC', 'Acre',
		IF(UF='BR-AL', 'Alagoas', 
		IF(UF='BR-BA', 'Bahia', 
		IF(UF='BR-GO', 'Goiás', 
		IF(UF='BR-PB', 'Paraíba',
		IF(UF='BR-SE', 'Sergipe',''))))))))))))))))))))))))))) as ESTADO

	FROM [lib://Base de Dados Empresa Hermex Log/base-de-dados-Hermex.xlsx]
	(ooxml, embedded labels, table is Pedidos);

![image](https://github.com/PedroMoeziaJr/Hermex-Log-Projeto-de-BI/assets/112977342/e643d4b4-2e3c-4868-b045-3ecbe8721eff)

|DAR| Qlik Sense


1-Mostrar as entregas feitas dentro do prazo.

-Criei a medida com a seguinte fórmula:
	
	Count(IF(Date([Data de Entrega]) <= Date([Data Prevista]), 1, 0))

>Date([Data de Entrega]): Essa parte da função converte o valor da coluna "Data de Entrega" em um objeto de data no formato correto. Isso permite que você compare e faça cálculos com datas.

>Date([Data Prevista]): Da mesma forma, essa parte converte o valor da coluna "Data Prevista" em um objeto de data.

><=: O operador <= verifica se a data de entrega é menor ou igual à data prevista. Ele retorna verdadeiro se a data de entrega estiver no prazo ou antecipada.

>IF(..., 1, 0): Essa parte do código usa a função condicional IF para atribuir o valor 1 se a condição anterior for verdadeira (ou seja, se a entrega estiver no prazo ou antecipada) e o valor 0 se for falsa (ou seja, se a entrega estiver atrasada).

>Count(...): A função Count é usada para contar o número de ocorrências do valor 1 resultante da função condicional acima. Isso contará o número de entregas que foram feitas dentro do prazo ou antecipadas.
Portanto, a função Count(IF(Date([Data de Entrega]) <= Date([Data Prevista]), 1, 0)) conta o número de entregas que foram feitas dentro do prazo ou antecipadas com base nas colunas "Data de Entrega" e "Data Prevista".


2-Mostrar entregas feitas em atraso.
-Criei a seguinte medida:
	
	Sum(IF(Date([Data de Entrega]) > Date([Data Prevista]), 1, 0))

>Date([Data de Entrega]): Essa parte da expressão converte o valor da coluna "Data de Entrega" em um objeto de data no formato correto. Isso permite que você compare e faça cálculos com datas.

>Date([Data Prevista]): Da mesma forma, essa parte converte o valor da coluna "Data Prevista" em um objeto de data.

>>: O operador > verifica se a data de entrega é maior que a data prevista. Ele retorna verdadeiro se a data de entrega estiver atrasada.

>IF(..., 1, 0): Essa parte do código usa a função condicional IF para atribuir o valor 1 se a condição anterior for verdadeira (ou seja, se a entrega estiver atrasada) e o valor 0 se for falsa (ou seja, se a entrega estiver dentro do prazo ou antecipada).

>Sum(...): A função Sum é usada para somar os valores resultantes da função condicional acima. Isso calcula a soma de todas as entregas que estão atrasadas.

Portanto, a expressão Sum(IF(Date([Data de Entrega]) > Date([Data Prevista]), 1, 0)) soma o número de entregas que estão atrasadas com base nas colunas "Data de Entrega" e "Data Prevista".

3-Quantidade de veículos disponíveis
Criei a seguinte medida:
	
	Count(DISTINCT [ID Veículo]) - Count(DISTINCT IF(Status = 'Em Uso', [ID Veículo]))

>Count(DISTINCT [ID Veículo]): Essa parte da expressão conta a quantidade distinta de valores na coluna "ID Veículo". Ela retorna o número total de veículos disponíveis, sem levar em conta o status.

>Count(DISTINCT IF(Status = 'Em Uso', [ID Veículo])): Nessa parte, utilizamos a função condicional IF para verificar se o status do veículo é igual a "Em Uso". Se a condição for verdadeira, ou seja, se o veículo estiver em uso, o valor [ID Veículo] é considerado para o cálculo. Caso contrário, não é considerado.

>Count(DISTINCT [ID Veículo]) - Count(DISTINCT IF(Status = 'Em Uso', [ID Veículo])): Subtraímos o número de veículos em uso do número total de veículos disponíveis. Essa subtração nos fornece a quantidade de veículos disponíveis que não estão em uso.

Portanto, a expressão Count(DISTINCT [ID Veículo]) - Count(DISTINCT IF(Status = 'Em Uso', [ID Veículo])) calcula a diferença entre o número total de veículos disponíveis e o número de veículos em uso, resultando no número de veículos disponíveis no momento.

4-Mostrar o tempo médio da compra até a data de expedição em um gráfico de linha, primeiro criei a medida: 

	Avg(Interval([Data de Entrega] - [Data da Compra], 'DD'))

>Avg(Interval([Data de Entrega] - [Data da Compra], 'DD'))[Data de Entrega] - [Data da Compra]: Essa parte da expressão calcula a diferença entre a data de entrega e a data da compra. O resultado dessa subtração será um valor numérico representando a diferença de dias entre as duas datas.

>Interval([Data de Entrega] - [Data da Compra], 'DD'): Utilizamos a função Interval para converter o resultado da subtração em um intervalo de tempo. Nesse caso, estamos especificando que queremos o intervalo em dias, usando a unidade 'DD'.

>Avg(Interval([Data de Entrega] - [Data da Compra], 'DD')): A função Avg é aplicada para calcular a média dos valores do intervalo de tempo em dias. Ela retorna o valor médio, considerando todos os registros.

>Portanto, a expressão Avg(Interval([Data de Entrega] - [Data da Compra], 'DD')) calcula a média do intervalo de tempo, em dias, entre a data de entrega e a data da compra. Essa média representa o tempo médio de expedição até a chegada do produto para o cliente.

5-Média de tempo entre compra e entrega (dias)

	Avg(Interval([Data de Entrega] - [Data da Compra], 'DD'))

>[Data de Entrega] - [Data da Compra]: Essa parte da expressão calcula a diferença entre a data de entrega e a data da compra. O resultado dessa subtração será um valor numérico representando a diferença de dias entre as duas datas.

>Interval([Data de Entrega] - [Data da Compra], 'DD'): Utilizamos a função Interval para converter o resultado da subtração em um intervalo de tempo. Nesse caso, estamos especificando que queremos o intervalo em dias, usando a unidade 'DD'.

>Avg(Interval([Data de Entrega] - [Data da Compra], 'DD')): A função Avg é aplicada para calcular a média dos valores do intervalo de tempo em dias. Ela retorna o valor médio, considerando todos os registros.

Portanto, a expressão Avg(Interval([Data de Entrega] - [Data da Compra], 'DD')) calcula a média do intervalo de tempo, em dias, entre a data de entrega e a data da compra. Essa média representa o tempo médio de expedição até a chegada do produto para o cliente.
