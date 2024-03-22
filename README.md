# ETL Pipeline (Countries API)

Este é o projeto final de uma ETL (Extract, Transform, Load) para processamento de dados geográficos usando `Python` e `Pandas` com dados da `API` pública **[REST Countries](https://github.com/ricksoliveira/ToDo-App_front-end)**.

O objetivo é conseguir extrair dados interessantes referentes à status social e informações físicas dos países.

Ele inclui as etapas de análises iniciais, transformação dos dados em diferentes data-frames , tratamento de valores ausentes e exclusão de duplicadas. Assim como a criação de um banco de dados utilizando `SQLite` para o carregamento (Load) dos data-frames para as tabelas correspondentes.

<br/>

## O que você precisa para rodar ?

> - **[Python](https://www.python.org)** (versão 3.9 ou acima)
> - **[PIP](https://pip.pypa.io/en/stable/installation/)**
> - **Qualquer IDE** (recomendado [VSCode](https://code.visualstudio.com) com os plugins `Jupyter` e `Python`)
> - **Conexão com a internet** (Para acessar a API REST Countries)

<br/>

## Como instalar

> 1) Para instalar esse projeto você precisa clonar este repositório com o comando:
>
>   ```
> 	  git clone https://github.com/ricksoliveira/coderhouse_ETL.git
>   ```
>
> - Ou apenas faça o download do arquivo `ZIP` e abra o projeto com a sua IDE.
>
> 2) Esta ETL utiliza das seguintes bibliotecas:
>    - Pandas
>    - Plyer
>    - Datetime
>    - Requests
>    - SQlite3
>
> - Para instalá-las, é necessário antes, rodar os seguintes comandos:
>
>   ```
> 		pip install pandas
> 		pip install plyer
> 		pip install requests
> 		pip install sqlite3
>   ```

<br/>

## Como executar

> - Este projeto está salvo no formato de Notebook, então basta apenas rodar cada bloco de código em ordem, ou clicar no botão **Run All** para que todos os blocos sejam rodados na ordem certa, como mostra a foto abaixo:
>
>      ![image](https://github.com/ricksoliveira/coderhouse-python-58620/assets/68413884/0514cead-5a6e-4f0b-933e-96be371fcad5)
>
> - Alguns blocos gerarão um output abaixo do mesmo, como por exemplo:
>
>      ![image](https://github.com/ricksoliveira/coderhouse-python-58620/assets/68413884/857164c9-0257-478e-bbb8-b62711f3e6df)
>
> - Caso haja qualquer problema em qualquer etapa do processo, uma notificação de alerta aparecerá em sua tela, especificando, em 3 níveis, a seriedade e em qual etapa do processo ocorreu o erro.
>
> - Consulte e explore as tabelas resultantes.


<br/>

## Componentes e Explicação

> 1) **Alerta de Função**
>
>    A função de alerta do programa é responsável por notificar o usuário sobre erros durante a execução do programa.
>
>    Ela tem como parâmetro o nível do alerta, o nome do banco de dados e a etapa em que ocorreu o problema. Em seguida, ela registra a data e hora do problema ocorrido.
>
>    A função utiliza a biblioteca **Plyer** para exibir a notificação na área de trabalho do usuário por um período de tempo determinado. Isso permite que o usuário seja informado imediatamente sobre qualquer problema.
>
> 2) **Extração dos dados**
>
>    - Extração dos dados pela API através da requisição *HTTP GET*:
>
>   ```
>    	https://restcountries.com/v3.1/all
>   ```
>
>    - Análise prévia dos dados, para sabermos como são os dados que estamos lidando utilizando OS comandos `countries_df.columns` e `countries_df.info()`:
> 
>      ![image](https://github.com/ricksoliveira/coderhouse-python-58620/assets/68413884/d8d06649-7197-4e7b-a8c4-340eaae44b74)
> 
> 3) **Transformação dos dados**
>
>    - Os dados do Data-Frame `countries_df` são separados em três diferentes Data-Frames, cada um contendo informações específicas sobre os países. Os dados são extraídos das colunas relevantes do Data-Frame original, isso permite uma melhor manipulação e análise dos dados de acordo com sua natureza. São eles:
>
>      - **social_status_df**
>      - **borders_df**
>      - **region_time_df**
>
>      ![image](https://github.com/ricksoliveira/coderhouse-python-58620/assets/68413884/3a9d50fb-29cd-4335-8768-d40fe1c320c1)
>
>    - Como o Data-Frame `borders_df` e `social_status_df`contém dados nulos nas colunas `Border With `e `Capital`. Eles precisam ser tratados, um país não ter bordas com nenhum país, significa que ele é uma ilha, assim como um país que não tem capital, significa que é território internacional, então o próximo passo é substituir os dados `NaN`:
>
>      ![image](https://github.com/ricksoliveira/coderhouse-python-58620/assets/68413884/52974623-90a5-417b-b991-2d423199956e)
>
>    - As colunas `Capital`, `Continent`, `Timezone` e `Border with` estão como tipo `lista`,  para que seja possível remover dados duplicados, uma conversão deve ser feita e os Data-Frames ajustados para lidar com valores de lista, mantendo apenas o primeiro elemento de cada lista. Em seguida, as possíveis duplicatas são removidas de cada Data-Frame com base em conjuntos específicos de colunas para garantir a unicidade de cada entrada e evitar redundância nos dados.
>      - ***OBS: As colunas `Timezones` e `Border with` não são tratadas, uma vez que cada país pode ter mais de 1 fuso horário e fazer fronteira com mais de 1 país, se tratássemos essas colunas dessa maneira, iríamos perder dados.***
>
>      ![image](https://github.com/ricksoliveira/coderhouse-python-58620/assets/68413884/5319da10-7aa8-43b8-8ca1-a65ff901285d)
>
> 4) **Carregamento no Banco de dados (Load)**
>
>    - As duas funções abaixo `save_db` e `read_table` foram criadas para, respectivamente, salvar um Data-Frame em uma tabela no banco de dados **SQLite**, e receber uma consulta SQL e o nome do arquivo do banco de dados como entrada. Dentro da função, uma conexão com o banco de dados é estabelecida usando o método `sqlite3.connect()`, em seguida é realizada uma consulta SQL. O resultado da consulta é então lido em um Data-Frame pelo `Pandas` usando o método `pd.read_sql()`, a conexão com o banco de dados é fechada antes de retornar o Data-Frame resultante.
>
>      ![image](https://github.com/ricksoliveira/coderhouse-python-58620/assets/68413884/774be558-2308-42b9-8cbc-a28cc6af8031)
>
>    - Em seguida uma demonstração de dois tipos de consulta diferentes feitas pela função `read_table()`:
>
>      ![image](https://github.com/ricksoliveira/coderhouse-python-58620/assets/68413884/bc13bd87-6d2a-4b4c-8348-03061de583e3)
>
>      ![image](https://github.com/ricksoliveira/coderhouse-python-58620/assets/68413884/dd772173-89e4-43c6-9e1f-3089fe197b1c)
>
>      A primeira imagem acima mostra uma consulta da qual é pego apenas o `Nome` e a `Capital` dos países com população inferior à 10000, em ordem crescente de `População`.
>
>      Já a segunda imagem nos dá o `Nome` e a `Área` de todos os países que fazem fronteira com a **Itália**, em ordem decrescente de tamanho.
>
>    - Também é possível fazermos consultas um pouco mais complexas, como **Joins** nas tabelas, conforme imagens abaixo:
>
>      ![image](https://github.com/ricksoliveira/coderhouse-python-58620/assets/68413884/75e32f7d-94c0-4ee3-8448-04c476f29806)
>
>      ![image](https://github.com/ricksoliveira/coderhouse-python-58620/assets/68413884/0999f22f-07ac-476e-a3e1-763ccac5b7da)
>
>    - A primeira imagem acima mostra um **Join** entre as tabelas `Borders` e `Region_time` com a chave `Nome`, nos dando não só a área dos países que fazem fronteira com a **Itália**, mas também os fusos horários destes países, informação esta que está em outra tabela.
>
>      A segunda imagem mostra o mesmo **Join**, porém sendo usado com a função do `merge()` do `Pandas`. Primeiro, o Data-Frame `social_status_df` é mesclado com o `borders_df` usando a coluna comum `Name` como chave, criando o Data-Frame `joined1_df`. Em seguida, é repetido o processo e `joined1_df` é mesclado com `region_time_df`, utilizando a mesma chave, Resultando no Data-Frame final `joined2_df`, que contém todos os dados mesclados de todas as três tabelas.

<br/>

## Autores

- **Guilherme Tuyama**
- **Gustavo Marreiros**
- **Henrique Oliveira**
- **Marcelo Bezerra**

<br/>


## Obrigado !

> Agradecimento especial à [**CODER HOUSE**](https://coderhouse.com.br) pelo curso de `Python para Análise de Dados`.
