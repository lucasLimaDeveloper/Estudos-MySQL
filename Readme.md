#Estudo de MySQL

- CRIAR UM BANCO DE DADOS:
	create database nome_do_banco
	default character set utf8
	default collate utf8_general_ci;

- CRIAR UM TABELA JÁ COM OS CAMPOS:
	create table nome_da_tabela(
	id int not null auto_increment,
	nome varchar(20) not null,
	idade int,
	peso decimal(5, 2),
	primary key(id)
	)default charset=utf8;

- INSERINDO DADOS EM UMA TABELA:
	insert into nome_da_tabela values
	(default, 'Pedro', '26', '80.7'),
	(default, 'Maria', '58', '68.9'),
	(default, 'José', '47', '76.7');

- ADICIONANDO UMA COLUNA:
	alter table nome_da_tabela
	add column nome_da_coluna varchar(10) after nome_da_coluna_desejada; (para inserir após uma coluna específica)
ou	add column nome_da_coluna varchar(10) first; (para inserir em primeiro lugar)
ou	add column nome_da_coluna varchar(10);

- ELIMINANDO UMA COLUNA:
	alter table nome_da_tabela
	drop column nome_da_coluna;

- MODIFICANDO DADOS DE UMA COLUNA:
	alter table nome_da_tabela
	modify column nome_da_coluna varchar(30) not null default 'Estudo';

- RENOMEANDO UMA COLUNA:
	alter table nome_da_tabela
	change column nome_da_coluna novo_nome varchar(20);
- RENOMEANDO A TABELA:
	alter table nome_da_tabela
	rename to novo_nome;

- ADICIONANDO CHAVE PRIMÁRIA CASO ELA NÃO TENHA SIDO DEFINIDA NO create table:
	alter table nome_da_tabela
	add primary key(nome_da_coluna);

- APAGANDO A TABELA:
	drop table nome_da_tabela;

- MODIFICANDO LINHAS INCORRETAS:
	update cursos
	set coluna_alterada = 'DIGITE A NOVA INFORMAÇÃO'
	where chave_primária = 'NÚMERO DA CHAVE PRIMÁRIA QUE ESTIVER NA LINHA DA INFORMAÇÃO A SER ALTERADA';
ou	(caso hajam mais informações a serem alteradas na mesma linha, usa-se:)
	set coluna_alterada = 'NOVA INFORMAÇÃO', coluna_alterada = 'NOVA INFORMAÇÃO'
	where chave_primária = 'NÚMERO DA CHAVE DESSA LINHA';

- APAGANDO UM LINHA:
	delete from nome_da_tabela
	where chave_primária = 'NÚMERO DA CHAVE CORRESPONDENTE À LINHA A SER APAGADA';

- APAGANDO TODAS AS LINHAS DE UMA TABELA SEM ALTERAR AS PROPRIEDADES DELA:
	truncate nome_da_tabela;

- PARA CRIAR UM DUMP(BACKUP):
	Basta clicar em Server > Data export > Selecionar o banco de dados > Clicar em Export to self-Contained File > Start export

- ORDENANDO:
	select * from nome_da_tabela
	order by nome_da_coluna;
ou	order by nome_da_coluna desc; (para ordenar ao contrário)

- FILTRAGENS USANDO SELECT:
	(PROCURA NAS COLUNAS IDCURSO E NOME DA TABELA CURSOS ONDE O IDCURSO FOR > 10 E ORDENADO POR NOME)
	select idcurso, nome from cursos
	where idcurso > 10
	order by nome;

	(PROCURA NAS COLUNAS nome E ano DA TABELA cursos ONDE O ano ESTIVER ENTRE 2014 E 2016 ORDENADO PELO ANO E NOME)
	select nome, ano from cursos
	where ano between 2014 and 2016
	order by ano, nome;

	(PROCURA NAS COLUNAS NOME E DESCRICAO DA TABELA CURSOS ONDE O ANO ESTIVER EM 2014 E 2016 ORDENADO PELO ANO)
	select nome, descricao from cursos
	where ano in (2014, 2016)
	order by ano;

	(PROCURA NAS COLUNAS NOME, CARGA E TOTAULAS DA TABELA CURSOS ONDE A CARGA É > QUE 35 E O TOTAULAS < 30)
	select nome, carga, totaulas from cursos
	where carga > 35 and totaulas < 30;

	(PROCURA NAS COLUNAS NOME, CARGA E TOTAULAS DA TABELA CURSOS ONDE A CARGA É > QUE 35 OU O TOTAULAS < 30)
	select nome, carga, totaulas from cursos
	where carga > 35 or totaulas < 30;

	(PROCURA EM TODAS AS COLUNAS DA TABELA CURSOS ONDE O NOME COMEÇAR COM 'A' E FOR SEGUIDO DE QUALQUER OUTRA COISA)
	select * from cursos
	where nome like 'A%';
ou	where nome like '%A'; (para procurar nomes terminados em A)
ou	where nome like '%A%'; (para procurar nomes que contenham A em qualquer posição)
ou 	where nome not like '%A%'; (para procurar nomes que NÃO contenham a letra A)

	(CAPTURAR DADOS DE FORMA DISTINTA, MESMO QUE O VALOR SE REPITA)
	select distinct carga from cursos;

	(AGRUPAR OS DADOS REPETIDOS EM UM ÚNICO VALOR, MAS DEIXANDO EM ABERTO A CONTAGEM DE QUANTOS FORAM CASO SEJA SOLICITADO)
	select carga from cursos
	group by carga;
	
	(CONTAGEM DOS VALORES SOLICITADOS)
	select carga, count(nome) from cursos
	group by carga;
	
	(ESPECIFICAR UMA CONDIÇÃO PARA SELECIONAR OS VALORES APENAS DO 'GROUP BY')
	select carga, count(nome) from cursos
	group by carga
	having count(nome) > 3; (Aqui ele irá selecionar a carga agrupada por carga tendo mais de 3 nomes agrupados)

- MODELO RELACIONAL:
	(CRIANDO UMA CHAVE ESTRANGEIRA PARA RELACIONAR DUAS TABELAS)
	alter table alunos
	add foreign key(cursopreferido)
	references cursos(idcursos); (Ao criar a chave estrangeira 'cursopreferido', referenciamos à coluna 'idcursos')

	(ATRIBUINDO UMA RELAÇÃO ENTRE DIFERENTES REGISTROS)
	update alunos
	set cursopreferido = '6'
	where id = '1'; 
	
	(FAZENDO COM QUE OS REGISTROS CORRESPONDAM À COLUNA RELACIONADA)
	select a.nome, c.nome, c.ano
	from alunos as a inner join cursos as c
	on c.idcurso = a.cursopreferido; (Aqui utilizamos o 'as' para apelidar nossa tabela alunos de 'a' e cursos de 'c')
ou	from alunos as a left join cursos as c (Nesse caso especificamos o 'left' para dar preferência ao que está á esquerda do join, ou seja, alunos)
ou	from alunos as a right join cursos as c (Já aqui utilizamos o 'right' para dar preferência a cursos)
