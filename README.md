# **Análise de Dados do Covid-19 - MySQL**

 Este projeto tem como objetivo realizar uma análise de dados utilizando o Sistema de Gerenciamento de Banco de Dados (SGBD) **MySQL**, com foco na pandemia do Covid-19, utilizando técnicas de estatísticas, agregações, relacionamentos e análise de dados ao longo do tempo. O objetivo principal é explorar dados reais relacionados à pandemia, buscando identificar padrões, tendências e insights relevantes. O **MySQL** será empregado como a ferramenta principal para armazenamento, manipulação e consulta dos dados. A análise abrangerá aspectos epidemiológicos, como casos confirmados, novos casos, percentual e média de mortes por região, bem como outras métricas relevantes. 
 Os dados utilizados neste projeto estão disponíveis publicamente neste [link](https://ourworldindata.org/covid-deaths).

<center>
<img src="https://www.simplilearn.com/ice9/free_resources_article_thumb/difference_between_sql_and_mysql.jpg">

---

# **Tópicos**

<ol type="1">
  <li>Dados;</li>
  <li>Criando o Schema e as tabelas no MySQL;</li>
  <li>Explorando os dados;</li>
  <li>Conclusão.</li>
</ol>

---

#  1\. **Dados**

 Os dados representam informações de mortes e vacinação global durante a pandemia do Covid-19, entre 01/01/2020 e 29/06/2021, e estão divididos em 2 arquivos no formato csv: covid_mortes e covid_vacinacao.

---

#  2\. **Criando o Schema e as tabelas no MySQL**

### **2.1. Criando o Schema dbcovid:**

Um schema é uma estrutura lógica que organiza e agrupa objetos relacionados, como tabelas, exibições, procedimentos armazenados, funções e outros elementos do banco de dados. Também é conhecido como banco de dados. Ao criar um novo banco de dados no MySQL, um schema é automaticamente criado com o mesmo nome do banco de dados, essa é a estrutura básica na qual pode-se criar e organizar as tabelas, exibições e outros objetos do banco de dados.

![schema](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/01%20-%20Criando%20o%20Schema%20dbcovid.png?raw=true)

---

### **2.2. Criando a Tabela 1 (covid_mortes):**

![tabela1](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/02%20-%20Criando%20a%20tabela%20covid_mortes.png?raw=true)

---

### **2.3. Criando a Tabela 2 (covid_vacinacao):**

![tabela2](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/03%20-%20Criando%20a%20tabela%20covid_vacina%C3%A7%C3%A3o.png?raw=true)

**Obs:**
-  Os dados das tabelas foram carregados via linha de comando através do cmd, pois como há uma grande quantidade de dados nos arquivos, o processo de carregamento via linha de comando é bem mais rápido;
-  Todas as colunas das 2 tabelas foram criadas com o tipo **"text"** em um primeiro momento para evitar erros ao carregar os dados, e serão modificados de acordo com a necessidade,  convertendo o tipo de cada coluna (int, bigint, double, varchar(25), etc) no decorrer do projeto, a medida que for necessário para realizar alguma operação matemática.

---

#  3\. **Explorando os dados**

Quando queremos visualizar todas as colunas de uma tabela, basta usar o asterisco (*) antes do **FROM**, assim todas as colunas existentes na tabela serão exibidas. Para retornar uma ou mais colunas específicas, basta escrever o nome da coluna no lugar do asterisco, adicionando uma virgula (,) após o nome da coluna caso tenha mais de uma, como veremos em exemplos mais a frente.

### **3.1. Visualizando os dados da tabela covid_mortes:**

![covid_mortes](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/04%20-%20Visualizando%20os%20dados%20covid_mortes.png?raw=true)

---

### **3.2. Visualizando os dados da tabela covid_vacinacao:**

![covid_vacinacao](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/05%20-%20Visualizando%20os%20dados%20covid_vacina%C3%A7%C3%A3o.png?raw=true)

**Obs:**
-  Algumas colunas de ambas as tabelas estão em branco pois no início da pandemia ainda não existiam registros, ou os dados não tinham sido publicados na data em questão.

  ---

### **3.3. Convertendo a coluna **date** para o formato de data com o comando str_to_date():**

 Através de uma instrução DML (Data Manipulation Linguage) a coluna "date" será convertida do formato "text" para o formato de data. O comando str_to_date converte a coluna para  formato de data, retornando o formato estipulado dentro dos parenteses, a documentação de referência pode ser acessada clicando neste [link](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html).

 **Obs:**
-  Por não possuir nenhum tipo de filtro, todas as linhas da coluna são modificadas após o uso dessa instrução DML, por isso, em alguns SGBD's, as cláusulas UPDATE e DELETE sem filtro são bloqueadas por padrão, pois a cláusula pode modificar completamente os dados, podendo ser recuperado somente via Backup, ou tentando uma nova conversão para o estado anterior da coluna, por esse motivo será preciso desbloquear o SAFE UPDATE, utilizando um comando do próprio MySQL (SET SQL_SAFE_UPDATES), e logo após a conversão da coluna, voltamos o SAFE UPDATE ao seu padrão original através do mesmo comando.

 Comando para desbloquear o SAFE UPDATE:
- SET SQL_SAFE_UPDATES = 0;

Comando para bloquear novamente o SAFE UPDATE:
- SET SQL_SAFE_UPDATES = 1;

![str_to_date](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/06%20-%20Convertendo%20as%20colunas%20de%20data%20-%20str%20para%20date.png?raw=true)

---

### **3.4. Selecionando as colunas mais relevantes da tabela covid_mortes para a análise:**

![colunas_relevantes](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/07%20-%20Selecionando%20colunas%20relevantes%20para%20a%20an%C3%A1lise%20dos%20dados.png?raw=true)

---

### **3.5. Análise Univariada:**

 Uma análise univariada examina e analisa uma única variável em um conjunto de dados, geralmente, essa análise é realizada para entender a distribuição, as estatísticas descritivas e as tendências dessa variável específica. Algumas das principais operações que podem ser em uma análise univariada no MySQL São: 
- Contagem de registros: COUNT();
- Estatísticas descritivas: AVG(), MEDIAN(), STDDEV(), MIN() e MAX();
- Análise de tendências: YEAR(), MONTH(), DAY().

Na query abaixo foi calculada a média de mortos de cada país utilizando a função **AVG()** na coluna **total_deaths**, agrupando os dados pela coluna **location**, retornando assim a média de mortos de cada país. O mesmo processo foi feito na coluna **new_cases** para retornar a média dos novos casos de cada país. 

A função **ROUND()** arredonda um valor numérico para um número específico de casas decimais, nesse caso foi arredondado para duas casas decimais.

![análise_univariada](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/08%20-%20Fazendo%20uma%20anal%C3%ADse%20univariada.png?raw=true)

---

### **3.6. Análise Multivariada:**

 Uma Análise multivariada examina e analisa a relação entre duas ou mais variáveis em um conjunto de dados. Diferentemente da análise univariada, que foca em uma única variável, a análise multivariada permite explorar as interações, associações e correlações entre múltiplas variáveis. Existem várias técnicas e operações disponíveis para realizar análises multivariadas. Algumas das principais são: 
- Matriz de correlação;
- Análise de regressão;
- Análise de clusterização;
- Análise de componentes principais (PCA);
- Análise de associação.

Na query abaixo foi feita uma divisão direta entre as colunas **total_deaths** e **total_cases**, multiplicando o resultado por 100 para achar o percentual de mortes em relação ao total de casos. Foram filtrados somente os dados do Brasil com a cláusula WHERE, ordenando o resultado pelas colunas **location** e **date**.

![análise_multivariada](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/09%20-%20Fazendo%20uma%20anal%C3%ADse%20multivariada.png?raw=true)

---

### **3.7. Proporção média entre o total de casos e a população de cada localidade:**

Na query abaixo foi calculado a média entre o total de casos e a população, dividindo a coluna **total_cases** pela coluna **population**, e multiplicado o resultado por 100 para encontrar a proporção média entre o total de casos e a população. O resultado foi agrupado pela localidade, encontrando o percentual médio de casos de cada país.

![proporção_média](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/10%20-%20Propor%C3%A7%C3%A3o%20m%C3%A9dia%20entre%20o%20total%20de%20casos%20e%20a%20popula%C3%A7%C3%A3o.png?raw=true)

---

### **3.8. Países com a maior taxa de infecção em relação a população:**

Na query abaixo foi utilizada a função **MAX()** na coluna **total_cases** que retorna o valor máximo do total de casos de infectados, e para achar o percentual máximo do total de casos pelo total da polulação de cada país, foi utilizada a função **MAX()** na divisão das colunas **total_cases** e **population**, multiplicando o resultado por 100. Também foi utilizado um filtro com a cláusula **WHERE** e **IS NOT NULL** para eliminar as linhas com valor nulo na coluna **continent**. O resultado foi agrupado pelas colunas **location** e **population** em ordem decrescente.

![taxainfeccao](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/11%20-%20Maior%20valor%20total%20casos,%20Taxa%20infec%C3%A7%C3%A3o%20x%20Popula%C3%A7%C3%A3o.png?raw=true)

---

### **3.9. Países com a maior número de mortes:**

Na query abaixo foi utilizada a função **MAX()** na coluna **total_deaths** para retornar o maior número de mortes, utilizando a cláusula **WHERE** e **IS NOT NULL** como na query acima, e agrupando pela coluna **location** para retornar o maior número de mortes de cada país. 

Note que nessa agregação em específico foi necessário utilizar a cláusula **CAST(nome_coluna AS UNSIGNED)**, isso porque quando a tabela foi criada, todas as colunas foram geradas como tipo **text**, e para realizar uma operação matemática foi necessário a conversão da coluna para um valor inteiro. Por padrão no SGBD MySQL, não é possível converter a coluna para o tipo **INT** utilizando essa cláusula, somente para o tipo UNSIGNED, que é basicamente o tipo **INT** sem sinal.

![maiornummortos](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/12%20-.png?raw=true)

---

### **3.10. Qual o percentual de mortes por dia:**

Na query abaixo foi utilizada a função **SUM()** na coluna **new_cases** para somar o número de novos casos de covid a cada dia, e retornar a soma de novos casos. Na coluna **total_deaths** foram feitos dois cálculos, uma soma para retornar o total de mortos, e a soma da coluna **new_deaths** dividida pela soma da coluna **new_cases**, e o resultado foi multiplicado por 100 para encontrar o percentual de mortes por dia. Nessa coluna foi utilizada a cláusula **COALESCE()**, que verifica os valores de uma coluna, e caso ela encontre um valor **NULL**, substitui pelo valor especificado entre parenteses, nesse caso 'NA', uma maneira eficiente de tratar valores nulos.

![percentmortes](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/13%20-%20Usando%20a%20fun%C3%A7%C3%A3o%20COALESCE%20para%20tratar%20dados%20nulos.png?raw=true)

---

### **3.11. Número de novos vacinados, e média móvel de novos vacinados ao longo do tempo por localidade:**

Na query abaixo, além da função **AVG()** utilizada na coluna **new_vaccinations** para encontrar a média de novos vacinados, foi utilizado a cláusula **OVER()**, particionando os dados pela localização, e ordenado pela data, para retornar a média móvel, ou seja, ao longo do tempo. 

Em resumo, a cláusula "OVER" é usada em consultas SQL para realizar cálculos agregados ou operações de janela (window functions). Ela permite realizar cálculos em um conjunto de linhas retornadas por uma consulta. É útil quando é necessário realizar cálculos que envolvam a combinação de várias linhas, como cálculos cumulativos, médias móveis, classificações e particionamento de dados, como foi feito utilizando o **PARTITION BY**, para particionar os resultados pela localização, e ordenar pela data, com o **ORDER BY**.

A query retorna o número de vacinados, e a média móvel de novos vacinados, considerando os dados somente da América do Sul, através da cláusula **WHERE**. 
Outras duas cláusulas usadas na query foram o **INNER JOIN**, e **CASE**.

O **INNER JOIN** é uma cláusula utilizada em SQL para combinar dados de várias tabelas com base em uma condição de junção específica, retornando apenas as linhas que correspondem nas tabelas envolvidas na junção. Essa cláusula permite que você recupere dados relacionados de tabelas diferentes com base em uma coluna comum entre elas. Nesse caso foram utilizadas as colunas **location** e **date** para relacionar as duas tabelas.

Já o **CASE** é uma expressão condicional usada em consultas SQL para realizar avaliações condicionais e retornar valores com base nessas condições. 

Nessa query a expressão foi utilizada para verificar o resultado de cada linha da coluna **new_vaccinations**, se ela encontrar algum valor nulo (NULL) ou em branco, substitui por 'NA', caso não encontre, retorna o valor encontrado na consulta.

![mediamovel](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/14.png?raw=true)

---

### **3.12. Criando uma tabela temporária com a cláusula **WITH** usando Common Table Expressions (CTE):**

Na query abaixo, criada para encontrar o percentual da população com pelo menos 1 dose da vacina ao longo do tempo, foi feito uma operação matemática, atribuindo um apelido (TotalNovosVacinados) para encontrar o total de novos vacinados ao longo do tempo, porém, ela não pode ser usada como uma coluna ao realizar uma nova operação matemática, pois é um apelido atribuido a uma operação ao longo do tempo, e não uma coluna nativa da tabela. 

Para resolver esse problema, usamos o CTE (Common Table Expressions) com a cláusula **WITH** para criar uma tabela temporária na memória do computador (DadosCovid), que retornará as colunas especificadas dentro dos parenteses, dessa forma, é possível utilizar a coluna **TotalNovosVacinados** em uma operação aritmética, e encontrar o percentual da população com pelo menos uma dose da vacina ao longo do tempo, como foi feito na operação apelidada de Percentual_1_Dose. Note que na query dentro dos parenteses já havia um filtro para retornar somente os dados do Brasil, e depois foi atribuido um outro filtro na coluna **date**, mudando a granularidade dos dados para mensal, e retornando somente os dados de Maio de 2021. 

![tabelatemp](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/15%20-%20Usando%20a%20fun%C3%A7%C3%A3o%20With%20para%20criar%20uma%20tabela%20tempor%C3%A1ria.png?raw=true)

---

### **3.13. Salvando ou substituindo uma query dentro do Banco de Dados:**

Uma forma de armazenar a query dentro do próprio banco de dados para facilitar a manutenção do código, e a apresentação do resultado final, é criando uma view dentro do próprio banco de dados, e que pode também ser substituida posteriormente com a cláusula **CREATE OR REPLACE VIEW**. Uma **VIEW** é uma representação virtual de uma tabela composta por uma consulta SQL. Ela permite que você defina uma consulta complexa como uma exibição nomeada, que pode ser referenciada posteriormente em outras consultas, como se fosse uma tabela real.

![salvandoview](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/16%20-%20Salvando%20ou%20substituindo%20a%20query%20dentro%20do%20banco%20de%20dados%20(View).png?raw=true)

---

### **3.14. Apresentando os dados através da view:**

![retornandoview](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/17%20-%20Visualizando%20os%20dados%20atrav%C3%A9s%20da%20view.png?raw=true)

---

### **3.15. Adicionando filtro em uma view:**

Também é possível adicionar filtros em uma view com a cláusula **WHERE**, isso permite restringir os dados retornados pela exibição com base em determinadas condições.

Na query abaixo foi adicionado um filtro para retornar somente os percentuais com valor acima dos 20%. 

![filtroview](https://github.com/jaquessonoliveira/MySQL-Projeto-analise-covid/blob/main/Arquivos/18%20-%20Criando%20um%20filtro%20dentro%20da%20View.png?raw=true)

---

#  4\. **Conclusão**

Com a análise exploratória dos dados do Covid-19 foi possível extrair os seguintes insights:

- Média de mortos por país,
- Proporção de mortes em relação ao total de casos no Brasil;
- Proporção média entre o total de casos e a população de cada localidade;
- Países com a maior taxa de infecção em relação à população (considerando o maior valor do total de casos);
- Países com o maior número de mortes;
- Percentual de mortes por dia;
- Número de novos vacinados;
- Média móvel de novos vacinados ao longo do tempo por localidade;
- Número de novos vacinados e o total de novos vacinados ao longo do tempo por continente;
- Percentual da população com pelo menos 1 dose da vacina ao longo do tempo em Maio de 2021.

---
