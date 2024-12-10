create database if not exists biblioteca_dados;
use biblioteca_dados;

select * from biblioteca_login;

CREATE TABLE biblioteca_login(
	id	INT PRIMARY KEY AUTO_INCREMENT,
	usuario	VARCHAR(100) NOT NULL UNIQUE,
	senha	VARCHAR(100) NOT NULL
);

create table usuario(
	id int primary key auto_increment,
    nome varchar(255),
    datanascimento varchar(8),
    email varchar(255),
    endereco varchar(255),
    cpf int(11) unique,
    telefone int(11)
);

 create table autor(
	id int primary key auto_increment,
    nome_autor varchar(255)
);

create table livro(
	id int primary key auto_increment,
    titulo varchar(255),
    subtitulo  varchar(255),
    editora varchar(255),
    datapublicacao varchar(8),
    isbn int(13),
    obs varchar(255),
    id_autor int,
    foreign key (id_autor) references autor(id)
);

create table empedev(
	id int primary key auto_increment,
	usuario varchar (255), 
    livro varchar (255), 
    dataemprestimo varchar (255), 
    dataprevisao varchar (15) ,
    datadevolucao varchar (15)
);
 
 Create View DIAS_PREVISAO as 
SELECT 
    id AS ID, 
    usuario AS Nome_do_Usuário, 
    livro AS Nome_do_Livro, 
    dataemprestimo AS Data_de_Empréstimo,
    CASE 
        WHEN STR_TO_DATE(dataprevisao, '%d/%m/%Y') < CURRENT_DATE THEN DATEDIFF(CURRENT_DATE, STR_TO_DATE(dataprevisao, '%d/%m/%Y'))
    END AS Dias_de_Atraso
FROM 
    empedev
WHERE 
    datadevolucao = '  /  /    '
HAVING
	Dias_de_Atraso > 0;
    
create view empativ as
select id as ID,
 usuario as Nome_do_Usuário, livro as Nome_do_Livro, 
dataemprestimo as Data_de_Empréstimo,
datadevolucao as Data_de_Devolução ,
dataprevisao as Data_de_Previsao
 from empedev
WHERE 
    datadevolucao = '  /  /    ';
