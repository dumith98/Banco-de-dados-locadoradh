/*
Meu projeto é a criação de um banco de dados de uma locadora. Usei as informações do ultimo checkpoint além de ter feito algumas correções. Esse BD guarda as informações dos filmes e séries que a locadora tem, seus clientes e os alugueis.
Cada aluguel possui o ID do cliente, o ID do filme e a data na qual o aluguel foi feito. Cada filme e série tem seus nomes, IDs, generos, diretores e data de lançamento.
Cada cliente tem nome, ID, contato telefonico, email e data de nascimento. Por fim, as entidades alguelfilme e aluguelserie mantem armazenado quem alugou qual filme e série registrando o cliente_id, filme_id ou serie_id e a data.
*/



CREATE SCHEMA locadoraDH;
USE locadoraDH;

/*Criação da Tabela*/
CREATE TABLE cliente(
cliente_id INT PRIMARY KEY AUTO_INCREMENT,
cliente_nome VARCHAR(50),
cliente_nasc DATE,
cliente_email CHAR(25),
cliente_contato CHAR(12)
);

CREATE TABLE genero (
genero_id TINYINT PRIMARY KEY AUTO_INCREMENT,
genero_nome VARCHAR(20)
);

CREATE TABLE diretor(
diretor_id INT PRIMARY KEY AUTO_INCREMENT,
diretor_nome VARCHAR(50),
diretor_sobrenome VARCHAR(50)
);

CREATE TABLE filme(
filme_id INT PRIMARY KEY AUTO_INCREMENT,
filme_nome VARCHAR (30),
unidades_disponiveis TINYINT,
filme_data DATE,
diretor_id INT,
genero_id TINYINT,
CONSTRAINT FKdiretor
FOREIGN KEY (diretor_id)
REFERENCES diretor(diretor_id),
CONSTRAINT FKgenero
FOREIGN KEY (genero_id)
REFERENCES genero(genero_id)
);

CREATE TABLE serie(
serie_id INT PRIMARY KEY AUTO_INCREMENT,
serie_nome VARCHAR(30) NOT NULL,
unidades_disponiveis TINYINT NOT NULL,
serie_data DATE NOT NULL,
diretor_id INT NOT NULL,
genero_id TINYINT NOT NULL,
CONSTRAINT FKdiretorserie
FOREIGN KEY (diretor_id)
REFERENCES diretor(diretor_id),
CONSTRAINT FKgeneroserie
FOREIGN KEY (genero_id)
REFERENCES genero(genero_id)
);

CREATE TABLE aluguelfilme(
aluguel_id INT PRIMARY KEY AUTO_INCREMENT,
cliente_id INT NOT NULL,
filme_id INT NOT NULL,
data_aluguel DATE,
CONSTRAINT FKcliente
FOREIGN KEY (cliente_id)
REFERENCES cliente(cliente_id),
CONSTRAINT FKfilme
FOREIGN KEY (filme_id)
REFERENCES filme (filme_id)
);

CREATE TABLE aluguelserie(
aluguel_id INT PRIMARY KEY AUTO_INCREMENT,
cliente_id INT NOT NULL,
serie_id INT NOT NULL,
data_aluguel DATE,
CONSTRAINT FKclienteserie
FOREIGN KEY (cliente_id)
REFERENCES cliente(cliente_id),
CONSTRAINT FKserie
FOREIGN KEY (serie_id)
REFERENCES serie (serie_id)
);

/* ALTER */
ALTER TABLE cliente
MODIFY cliente_nome VARCHAR(50) NOT NULL,
MODIFY cliente_nasc DATE NOT NULL,
MODIFY cliente_email CHAR(25) NOT NULL,
MODIFY cliente_contato CHAR(12)NOT NULL;

ALTER TABLE genero
MODIFY genero_nome VARCHAR(20) NOT NULL;

ALTER TABLE diretor
MODIFY diretor_nome VARCHAR(50) NOT NULL,
MODIFY diretor_sobrenome VARCHAR(50) NOT NULL;

ALTER TABLE filme
MODIFY filme_nome VARCHAR (30) NOT NULL,
MODIFY unidades_disponiveis TINYINT NOT NULL,
MODIFY filme_data DATE NOT NULL,
MODIFY diretor_id INT NOT NULL,
MODIFY genero_id TINYINT NOT NULL;

ALTER TABLE aluguelfilme
MODIFY data_aluguel DATE NOT NULL;

ALTER TABLE aluguelserie
MODIFY data_aluguel DATE NOT NULL;

/* INSERT*/
INSERT INTO cliente(cliente_nome, cliente_nasc, cliente_email, cliente_contato) VALUES
	('Dumith Bou Habib', '1998-04-08', 'emaildodumith@email.com', 9939309332),
    ('Maisa Dare Perim', '1999-08-10', 'emaildamaisa@emal.com', 912345678),
    ('Aisha do Couto', '2003-04-16', 'emailerrado@email.com', 987654321);

INSERT INTO diretor(diretor_nome, diretor_sobrenome) VALUES
	('Peter', 'Jackson'),
    ('Denis', 'Villeneuve'),
    ('Sergio', 'Pablos');

INSERT INTO diretor(diretor_nome, diretor_sobrenome) VALUES
	('Ash', 'Brannon'),
    ('Matt', 'Duffer');

    
INSERT INTO genero(genero_nome) VALUES
	('Ação'),
    ('Suspense'),
    ('Comédia'),
    ('Terror'),
    ('Aventura'),
    ('Ficção Científica'),
    ('Drama'),
    ('Fantasia');
    
INSERT INTO genero(genero_nome) VALUES ('Natalino');


INSERT INTO filme(filme_nome, unidades_disponiveis, filme_data, diretor_id, genero_id) VALUES 
	('LOTR: A Sociedade do Anel', 110, '2002-1-1', 1, 8),
    ('LOTR: As Duas Torres', 100, '2002-12-27', 1, 8),
    ('LOTR: O Retorno do Rei', 50, '2003-12-25', 1, 8),
    ('DUNA', 110, '2021-10-21', 2, 6),
    ('Klaus', 125, '2019-11-08', 3, 9);

INSERT INTO serie(serie_nome, unidades_disponiveis, serie_data, diretor_id, genero_id) VALUES
	('Arcane', 100, '2021-11-06', 4, 1),
    ('Stranger Things', 90, '2016-07-15', 5, 2);
    
INSERT INTO aluguelserie(cliente_id, serie_id, data_aluguel) VALUES
	(3, 1, '2021-11-27'),
    (2, 2, '2021-11-27');
    
INSERT INTO aluguelfilme(cliente_id, filme_id, data_aluguel) VALUES
	(1, 1, '2020-12-20'),
    (1, 2, '2021-04-08'),
    (1, 3, '2021-08-10');
    
/* DELETE*/
DELETE FROM filme WHERE filme_nome = 'DUNA';

/* Update */
UPDATE cliente
SET cliente_email = 'emaildaaisha@email.com'
WHERE cliente_id = 3;


/* SELECTS */
SELECT * FROM aluguelfilme;
SELECT * FROM aluguelserie;

SELECT cliente_nome FROM cliente;

SELECT filme.filme_nome, genero.genero_nome FROM filme
INNER JOIN genero
ON genero.genero_id = filme.genero_id; 

SELECT serie.serie_nome, genero.genero_nome FROM serie
INNER JOIN genero
ON genero.genero_id = serie.genero_id; 

SELECT cliente.cliente_nome AS 'Nome do cliente', filme.filme_nome AS 'Nome do filme' FROM aluguelfilme
	INNER JOIN cliente
		ON aluguelfilme.cliente_id = cliente.cliente_id
	INNER JOIN filme
		ON aluguelfilme.filme_id = filme.filme_id;
        
SELECT cliente.cliente_nome AS 'Nome do cliente', serie.serie_nome AS 'Nome da serie' FROM aluguelserie
	INNER JOIN cliente
		ON aluguelserie.cliente_id = cliente.cliente_id
	INNER JOIN serie
		ON aluguelserie.serie_id = serie.serie_id;