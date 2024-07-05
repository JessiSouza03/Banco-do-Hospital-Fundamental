# Banco-do-Hospital-Fundamental
Atividade proposta pelo professor Gabriel - Elaborar as etapas de criação de um banco de dados para o Hospital Fundamental.

# Etapa 1

A primeira etapa foi a diagramação DER, é a primeira vez que utilizo a ferramenta BRModelo, também estou me adaptando novamente com as cardinalidades e estudando as com 0, não tive a chance de conhecê-lás antes, mas quero usar no meu próximo diagrama.
![diagramader](https://github.com/JessiSouza03/Banco-do-Hospital-Fundamental/assets/96502061/355fc251-bd94-4dfe-ba25-d08af2abfb97)
## Entidades:

1. **Paciente:** Representa os indivíduos que recebem tratamento médico no hospital. Cada paciente é identificado por um código único.

2. **Médico:** Representa os profissionais de saúde que realizam consultas e prescrevem tratamentos para os pacientes. Cada médico é identificado por um código único.

3. **Consulta:** Representa as interações entre médicos e pacientes, onde o diagnóstico e o tratamento são discutidos e prescritos. Cada consulta é identificada por um número único e está associada a um único paciente (cardinalidade "1..1") e a um ou mais médicos (cardinalidade "1..n").

4. **Convênio:** Representa os planos de saúde ou seguros médicos aos quais os pacientes podem estar afiliados. Cada convênio é identificado por um código único e pode ter zero ou mais pacientes associados a ele (cardinalidade "1..n").

5. **Receita:** Representa as prescrições médicas feitas durante uma consulta. Cada receita é identificada por um número único e está associada a uma única consulta (cardinalidade "1..1").

6. **Relatório de Receita:** Representa uma cópia ou registro da receita médica que é entregue ao paciente. Cada relatório de receita está associado a uma única receita (cardinalidade "1..1") e, por extensão, a uma única consulta e a um único paciente.

## Relacionamentos:

- **Paciente - Consulta:** Um paciente pode ter uma ou mais consultas, mas cada consulta está associada a apenas um paciente (cardinalidade "1..n" do lado do Paciente e "1..1" do lado da Consulta).

- **Médico - Consulta:** Cada consulta é realizada por um ou mais médicos, mas cada médico pode estar associado a várias consultas (cardinalidade "1..n").

- **Paciente - Convênio:** Cada paciente pode estar associado a zero ou um convênio, e cada convênio pode ter zero ou mais pacientes associados a ele (cardinalidade "0..1" do lado do Paciente e "0..n" do lado do Convênio).

- **Consulta - Receita:** Cada consulta resulta em uma prescrição médica, portanto, cada receita está associada a uma única consulta (cardinalidade "1..1").

  - **Receita - Relatório de Receita:** Cada receita tem um relatório correspondente que é entregue ao paciente, portanto, cada relatório de receita está associado a uma única receita (cardinalidade "1..1").

## Adicionando novas entidades e atributos:

- **Paciente - Internação:** Cada paciente está associado a uma ou mais internações (cardinalidade "1..n"), e cada internação está ligada a exatamente um paciente (cardinalidade "1").

- **Internação - Quarto:** Cada internação está associada a um único quarto (cardinalidade "1"), e cada quarto pode estar vinculado a várias internações ao longo do tempo (cardinalidade "n").

- **Quarto - TipoQuarto:** Cada quarto pertence a um tipo específico de quarto (cardinalidade "1"), definido na entidade TipoQuarto.

- **Enfermeiro(a) - Enfermeiro(a) na Internação:** Cada enfermeiro(a) pode estar relacionado a várias internações como responsável pelo acompanhamento (cardinalidade "n"), e cada internação tem um ou mais enfermeiros(as) designados (cardinalidade "n").

- **Internação - Médico:** Cada internação é supervisionada por um único médico (cardinalidade "1"), e cada médico pode ser responsável por várias internações ao longo do tempo (cardinalidade "n").


![Banci Hospital](https://github.com/JessiSouza03/Banco-do-Hospital-Fundamental/assets/96502061/b68c879d-83a7-4f56-b6fe-d14b34b7d954)

## Código SQL:
```ruby
create database hospital;
use hospital;

create table medico (
	codigo_medico int primary key,
	nome_medico varchar(50) not null,
	cpf_medico varchar(15) unique not null,
	rg_medico varchar(9) unique not null,
	cargo_medico varchar(20) not null, 
    crm_medico varchar(20) not null,
	data_nasc_medico date not null
);

create table especialidade_medico(
	codigo_medico int not null,
    codigo_especialidade int not null
);

create table paciente (
	codigo_paciente int primary key,
	nome_paciente varchar(50) not null,
	cpf_paciente varchar(11) unique not null, 
	rg_paciente varchar(9) unique not null, 
	telefone_paciente varchar(11) not null,
	email_paciente varchar(50) unique not null,
	data_nasc_paciente date not null,
	numero_carteira_convenio int not null
);

create table convenio (
	numero_carteira int primary key, 
	nome_convenio varchar(50) not null,
	tempo_carencia varchar(10) not null,
	cnpj_convenio varchar(14) unique not null
);

create table consulta(
	codigo_consulta int primary key,
	data_consulta date not null,
	horario_consulta time not null,
	valor_consulta decimal(5,2) not null,
	forma_pagamento varchar(20) not null,
	codigo_paciente int not null,
	codigo_medico int not null, 
	codigo_especialidade int not null,
    numero_carteira_convenio int
);

create table especialidade(
	codigo_especialidade int primary key,
	descricao_especialidade varchar(50) not null
);

create table receita(
	codigo_receita int primary key,
    nome_medicamento varchar(100) not null,
    instrucoes_medicamento varchar(255) not null,
    qtd_medicamento int not null,
    codigo_consulta int not null
);

create table endereco(
	codigo_endereco int primary key,
    rua varchar(100) not null,
    bairro varchar(100) not null,
    cep varchar(9) not null,
    numero int not null,
    complemento varchar(100),
    codigo_paciente int not null
);

create table internacao(
	codigo_internacao int primary key,
    data_prevista_alta date not null,
    data_entrada date not null,
    data_alta date,
    codigo_medico int not null,
    numero_quarto int not null,
    codigo_paciente int not null,
    numero_carteira_convenio int
);

create table quarto(
	numero_quarto int primary key,
    codigo_tipo_quarto int not null
);

create table procedimento(
	codigo_procedimento int primary key,
    nome_procedimento varchar(50) not null,
    descricao_procedimento varchar(100) not null
);

create table procedimento_internacao(
	codigo_internacao int not null,
    codigo_procedimento int not null
);

create table enfermeiro(
	codigo_enfermeiro int primary key,
    nome_enfermeiro varchar(50) not null,
    rg_enfermeiro varchar(9) unique not null,
    cpf_enfermeiro varchar(15) unique not null,
	cre_enfermeiro varchar(20) unique not null,
    data_nasc_enfermeiro date not null
);

create table tipo_quarto(
	codigo_tipo_quarto int primary key,
    diaria_quarto decimal(5,2) not null,
    descricao_quarto varchar(200) not null
);

create table enfermeiro_internacao(
	codigo_enfermeiro int not null,
    codigo_internacao int not null
);

/*relacionando convenio e paciente*/
alter table paciente add foreign key fk_numero_carteira_convenio (numero_carteira_convenio) references convenio(numero_carteira);

/*relacionando consulta e especialidade*/
alter table consulta add foreign key fk_codigo_especialidade (codigo_especialidade) references especialidade(codigo_especialidade);

/*relacionando médico e consulta*/
alter table consulta add foreign key fk_codigo_medico (codigo_medico) references medico(codigo_medico);

/*relacionando paciente e consulta*/
alter table consulta add foreign key fk_codigo_paciente (codigo_paciente) references paciente(codigo_paciente);

/*relacionando consulta e receita*/
alter table receita add foreign key fk_codigo_consulta (codigo_consulta) references consulta(codigo_consulta);

/*relacionando consulta e convenio*/
alter table consulta add foreign key fk_numero_carteira_convenio (numero_carteira_convenio) references convenio(numero_carteira);

/*relacionando paciente e endereço*/
alter table endereco add foreign key fk_codigo_paciente (codigo_paciente) references paciente(codigo_paciente);

/*relacionando quarto e internação*/
alter table internacao add foreign key fk_numero_quarto (numero_quarto) references quarto(numero_quarto);

/*relacionando internação e procedimento*/
alter table procedimento_internacao add foreign key fk_codigo_internacao (codigo_internacao) references internacao(codigo_internacao);
alter table procedimento_internacao add foreign key fk_codigo_procedimento (codigo_procedimento) references procedimento(codigo_procedimento);

/*relacionando internação e médico*/	
alter table internacao add foreign key fk_codigo_medico (codigo_medico) references medico(codigo_medico);

/*relacionando enfermeiro e internação*/
alter table enfermeiro_internacao add foreign key fk_codigo_enfermeiro (codigo_enfermeiro) references enfermeiro(codigo_enfermeiro);
alter table enfermeiro_internacao add foreign key fk_codigo_internacao (codigo_internacao) references internacao(codigo_internacao);

/*relacionando quarto e tipo*/
alter table quarto add foreign key fk_tipo_quarto (codigo_tipo_quarto) references tipo_quarto(codigo_tipo_quarto);

/*relacionando especialidade e médico*/
alter table especialidade_medico add foreign key fk_codigo_medico (codigo_medico) references medico(codigo_medico);
alter table especialidade_medico add foreign key fk_codigo_especialidade (codigo_especialidade) references especialidade(codigo_especialidade);

/*relacionando internação e paciente*/
alter table internacao add foreign key fk_codigo_paciente (codigo_paciente) references paciente(codigo_paciente);

/*relacionando internação e convênio*/
alter table internacao add foreign key fk_numero_carteira_convenio (numero_carteira_convenio) references convenio(numero_carteira);
```

## Alimentando o Banco:
- Inclua ao menos dez médicos de diferentes especialidades. Ao menos sete especialidades (considere a afirmação de que “entre as especialidades há pediatria, clínica geral, gastrenterologia e dermatologia”).

```sql
INSERT INTO especialidade (codigo_especialidade, descricao_especialidade) VALUES
(1, 'Cardiologia'),
(2, 'Pediatria'),
(3, 'Clínica geral'),
(4, 'Ortopedia'),
(5, 'Dermatologia'),
(6, 'Psiquiatria'),
(7, 'Ginecologia'),
(8, 'Oftalmologia'),
(9, 'Gastroenterologia'),
(10, 'Oncologia');

INSERT INTO medico (codigo_medico, nome_medico, cpf_medico, rg_medico, crm_medico, cargo_medico, data_nasc_medico, codigo_especialidade) VALUES
(1, 'Dr. Marcos Almeida', '123.456.789-00', 'MG1234567', 'CRM12345/MG', 'Cardiologista', '1970-04-10', 1),
(2, 'Dra. Julia Martins', '234.567.890-11', 'SP2345678', 'CRM23456/SP', 'Pediatra', '1983-07-12', 2),
(3, 'Dr. Roberto Mendes', '345.678.901-22', 'RJ3456789', 'CRM34567/RJ', 'Clínico Geral', '1975-10-15', 3),
(4, 'Dra. Paula Souza', '456.789.012-33', 'PR4567890', 'CRM45678/PR', 'Ortopedista', '1980-11-20', 4),
(5, 'Dr. Carlos Ferreira', '567.890.123-44', 'RS5678901', 'CRM56789/RS', 'Dermatologista', '1972-09-25', 5),
(6, 'Dra. Mariana Silva', '678.901.234-55', 'SC6789012', 'CRM67890/SC', 'Psiquiatra', '1985-01-30', 6),
(7, 'Dr. Fernando Lima', '789.012.345-66', 'BA7890123', 'CRM78901/BA', 'Ginecologista', '1978-05-22', 7),
(8, 'Dra. Camila Rocha', '890.123.456-77', 'DF8901234', 'CRM89012/DF', 'Oftalmologista', '1989-02-14', 8),
(9, 'Dr. Bruno Costa', '901.234.567-88', 'CE9012345', 'CRM90123/CE', 'Gastroenterologista', '1980-08-10', 9),
(10, 'Dra. Renata Oliveira', '012.345.678-99', 'GO0123456', 'CRM01234/GO', 'Oncologista', '1983-03-19', 10);
```

- Inclua ao menos 15 pacientes.
```sql
-- Convênios
INSERT INTO convenio (numero_carteira, nome_convenio, tempo_carencia, cnpj_convenio) VALUES
(1, 'Saúde Mais', '30 dias', '11122233000101'),
(2, 'Vida Boa', '60 dias', '22233344000202'),
(3, 'Bem Estar', '45 dias', '33344455000303'),
(4, 'Saúde Total', '30 dias', '44455566000404'),
(5, 'Plano Ouro', '60 dias', '55566677000505');

-- Pacientes
INSERT INTO paciente (codigo_paciente, nome_paciente, cpf_paciente, rg_paciente, telefone_paciente, email_paciente, data_nasc_paciente, numero_carteira_convenio) VALUES
(1, 'Marcelo Farias', '98765432100', 'MG7654321', '31987651234', 'marcelo.farias@example.com', '1991-01-11', 1),
(2, 'Luciana Mendes', '87654321011', 'SP8765432', '11987651235', 'luciana.mendes@example.com', '1986-02-21', 2),
(3, 'Felipe Costa', '76543210922', 'RJ7654323', '21987651236', 'felipe.costa@example.com', '1994-03-31', 3),
(4, 'Patrícia Ramos', '65432109833', 'PR6543214', '41987651237', 'patricia.ramos@example.com', '1981-04-14', 4),
(5, 'Thiago Lima', '54321098744', 'RS5432105', '51987651238', 'thiago.lima@example.com', '1976-05-25', 5),
(6, 'Carla Souza', '43210987655', 'SC4321096', '61987651239', 'carla.souza@example.com', '1989-06-06', 1),
(7, 'Fernando Moreira', '32109876566', 'BA3210987', '71987651240', 'fernando.moreira@example.com', '1980-07-17', 2),
(8, 'Aline Santos', '21098765477', 'DF2109878', '81987651241', 'aline.santos@example.com', '1993-08-18', 3),
(9, 'Bruno Oliveira', '10987654388', 'CE1098769', '91987651242', 'bruno.oliveira@example.com', '1984-09-29', 4),
(10, 'Rita Fernandes', '09876543299', 'GO0987650', '61987651243', 'rita.fernandes@example.com', '1990-10-10', 5),
(11, 'Paulo Rocha', '98765012345', 'MA9876501', '71987651244', 'paulo.rocha@example.com', '1986-11-21', 1),
(12, 'Mariana Lima', '87654101234', 'PA8765410', '81987651245', 'mariana.lima@example.com', '1981-12-22', 2),
(13, 'Rodrigo Martins', '76543210123', 'PE7654321', '91987651246', 'rodrigo.martins@example.com', '1994-01-23', 3),
(14, 'Sofia Ferreira', '65432101234', 'PI6543212', '61987651247', 'sofia.ferreira@example.com', '1995-02-24', 4),
(15, 'Vitor Gomes', '54321012345', 'RJ5432103', '31987651248', 'vitor.gomes@example.com', '1991-03-25', 5);

-- Endereços
INSERT INTO endereco (codigo_endereco, rua, bairro, cep, numero, complemento, codigo_paciente) VALUES
(1, 'Rua Alfa', 'Bairro Beta', '30123456', 201, 'Apto 1', 1),
(2, 'Rua Gama', 'Bairro Delta', '40123456', 202, 'Apto 2', 2),
(3, 'Rua Epsilon', 'Bairro Zeta', '50123456', 203, 'Apto 3', 3),
(4, 'Rua Eta', 'Bairro Theta', '60123456', 204, 'Apto 4', 4),
(5, 'Rua Iota', 'Bairro Kappa', '70123456', 205, 'Apto 5', 5),
(6, 'Rua Lambda', 'Bairro Mu', '80123456', 206, 'Apto 6', 6),
(7, 'Rua Nu', 'Bairro Xi', '90123456', 207, 'Apto 7', 7),
(8, 'Rua Omicron', 'Bairro Pi', '01123456', 208, 'Apto 8', 8),
(9, 'Rua Rho', 'Bairro Sigma', '11123456', 209, 'Apto 9', 9),
(10, 'Rua Tau', 'Bairro Upsilon', '12123456', 210, 'Apto 10', 10),
(11, 'Rua Phi', 'Bairro Chi', '13123456', 211, 'Apto 11', 11),
(12, 'Rua Psi', 'Bairro Omega', '14123456', 212, 'Apto 12', 12),
(13, 'Rua Beta', 'Bairro Alfa', '15123456', 213, 'Apto 13', 13),
(14, 'Rua Delta', 'Bairro Gama', '16123456', 214, 'Apto 14', 14),
(15, 'Rua Zeta', 'Bairro Epsilon', '17123456', 215, 'Apto 15', 15);
```

- Registre 20 consultas de diferentes pacientes e diferentes médicos (alguns pacientes realizam mais que uma consulta). As consultas devem ter ocorrido entre 01/01/2015 e 01/01/2022. Ao menos dez consultas devem ter receituário com dois ou mais medicamentos.

```sql
insert into consulta (codigo_consulta, data_consulta, horario_consulta, valor_consulta, forma_pagamento, codigo_paciente, codigo_medico, codigo_especialidade, numero_carteira_convenio) values
(1, '2023-05-15', '09:00:00', 150.00, 'Cartão', 1, 1, 1, 1),
(2, '2023-06-20', '10:30:00', 200.00, 'Dinheiro', 2, 2, 2, 2),
(3, '2023-07-25', '11:00:00', 175.00, 'Cartão', 3, 3, 3, 3),
(4, '2023-08-10', '08:45:00', 180.00, 'Cartão', 4, 4, 4, 4),
(5, '2023-09-05', '14:00:00', 160.00, 'Cartão', 5, 5, 5, 5),
(6, '2023-10-15', '15:30:00', 190.00, 'Cartão', 6, 6, 6, 1),
(7, '2023-11-20', '09:15:00', 170.00, 'Dinheiro', 7, 7, 7, 2),
(8, '2023-12-25', '10:00:00', 165.00, 'Cartão', 8, 8, 8, 3),
(9, '2024-01-30', '11:30:00', 185.00, 'Cartão', 9, 9, 9, 4),
(10, '2024-02-15', '13:00:00', 175.00, 'Dinheiro', 10, 10, 10, 5),
(11, '2024-03-10', '14:15:00', 200.00, 'Cartão', 11, 1, 1, 1),
(12, '2024-04-15', '15:45:00', 150.00, 'Dinheiro', 12, 2, 2, 2),
(13, '2024-05-20', '16:00:00', 170.00, 'Cartão', 13, 3, 3, 3),
(14, '2024-06-25', '17:30:00', 180.00, 'Cartão', 14, 4, 4, 4),
(15, '2024-07-30', '08:30:00', 160.00, 'Cartão', 15, 5, 5, 5),
(16, '2024-08-05', '09:45:00', 190.00, 'Cartão', 1, 6, 6, 1),
(17, '2024-09-10', '10:00:00', 165.00, 'Dinheiro', 2, 7, 7, 2),
(18, '2024-10-15', '11:15:00', 175.00, 'Cartão', 3, 8, 8, 3),
(19, '2024-11-20', '12:00:00', 185.00, 'Cartão', 4, 9, 9, 4),
(20, '2025-01-01', '13:45:00', 150.00, 'Dinheiro', 5, 10, 10, 5);

insert into receita (codigo_receita, nome_medicamento, qtd_medicamento, instrucoes_medicamento, codigo_consulta) values
(1, 'Dipirona', 1, 'Tomar 1 comprimido a cada 8 horas', 1),
(2, 'Vitamina C', 2, 'Tomar 1 comprimido ao dia', 2),
(3, 'Antialérgico', 1, 'Tomar 1 comprimido ao dia', 3),
(4, 'Ibuprofeno', 3, 'Tomar 1 comprimido a cada 8 horas', 4),
(5, 'Paracetamol', 1, 'Tomar 1 comprimido a cada 8 horas', 5),
(6, 'Metformina', 2, 'Tomar 1 comprimido 2 vezes ao dia', 6),
(7, 'Lorazepam', 1, 'Tomar 1 comprimido ao dormir', 7),
(8, 'Diclofenaco', 5, 'Tomar 1 comprimido a cada 8 horas', 8),
(9, 'Cetoconazol', 6, 'Aplicar o creme na área afetada 2 vezes ao dia', 9),
(10, 'Azitromicina', 2, 'Tomar 1 comprimido ao dia por 3 dias', 10),
(11, 'Atenolol', 2, 'Tomar 1 comprimido ao dia', 11),
(12, 'Prednisona', 1, 'Tomar 1 comprimido ao dia', 12),
(13, 'Aspirina', 8, 'Tomar 1 comprimido a cada 12 horas', 13),
(14, 'Clonazepam', 4, 'Tomar 1 comprimido ao dormir', 14),
(15, 'Sinvastatina', 4, 'Tomar 1 comprimido ao dia', 15),
(16, 'Nimesulida', 3, 'Tomar 1 comprimido a cada 8 horas', 16),
(17, 'Levotiroxina', 6, 'Tomar 1 comprimido ao dia', 17),
(18, 'Alprazolam', 3, 'Tomar 1 comprimido ao dormir', 18),
(19, 'Pantoprazol', 1, 'Tomar 1 comprimido ao dia', 19),
(20, 'Fluconazol', 1, 'Tomar 1 comprimido ao dia por 7 dias', 20);

insert into consulta (codigo_consulta, data_consulta, horario_consulta, valor_consulta, forma_pagamento, codigo_paciente, codigo_medico, codigo_especialidade, numero_carteira_convenio) values
(21, '2025-02-01', '09:00:00', 150.00, 'Dinheiro', 1, 1, 1, NULL),
(22, '2025-02-10', '11:00:00', 200.00, 'Cartão', 2, 2, 2, NULL),
(23, '2025-03-01', '13:00:00', 175.00, 'Cartão', 3, 3, 3, NULL),
(24, '2025-03-15', '15:00:00', 180.00, 'Dinheiro', 4, 4, 4, NULL);

insert into receita (codigo_receita, nome_medicamento, qtd_medicamento, instrucoes_medicamento, codigo_consulta) values
(21, 'Dipirona', 1, 'Tomar 1 comprimido a cada 6 horas', 21),
(22, 'Vitamina C', 2, 'Tomar 1 comprimido ao dia', 22),
(23, 'Antialérgico', 1, 'Tomar 1 comprimido ao dia', 23),
(24, 'Ibuprofeno', 3, 'Tomar 1 comprimido a cada 8 horas', 24);
```

- Registre ao menos sete internações. Pelo menos dois pacientes devem ter se internado mais de uma vez. Ao menos três quartos devem ser cadastrados. As internações devem ter ocorrido entre 01/01/2015 e 01/01/2022. Considerando que “a princípio o hospital trabalha com apartamentos, quartos duplos e enfermaria”, inclua ao menos esses três tipos com valores diferentes. Inclua dados de dez profissionais de enfermaria. Associe cada internação a ao menos dois enfermeiros.

```sql
insert into tipo_quarto (codigo_tipo_quarto, diaria_quarto, descricao_quarto) values
(1, 300.00, 'Apartamento'),
(2, 150.00, 'Quarto Duplo'),
(3, 100.00, 'Enfermaria');

insert into quarto (numero_quarto, codigo_tipo_quarto) values
(101, 1),
(102, 2),
(103, 3),
(104, 1);  -- Adicionando um novo quarto apartamento

insert into internacao (codigo_internacao, data_prevista_alta, data_entrada, data_alta, codigo_medico, numero_quarto, codigo_paciente, numero_carteira_convenio) values
(1, '2021-01-10', '2021-01-01', '2021-01-10', 1, 101, 1, 1),
(2, '2021-02-15', '2021-02-01', '2021-02-15', 2, 102, 2, 2),
(3, '2020-05-05', '2020-05-01', '2020-05-05', 3, 103, 3, 3),
(4, '2019-07-10', '2019-07-01', '2019-07-10', 4, 101, 4, 4),
(5, '2018-09-20', '2018-09-10', '2018-09-20', 5, 102, 5, 5),
(6, '2017-10-15', '2017-10-01', '2017-10-15', 6, 103, 1, 1),
(7, '2016-12-25', '2016-12-15', '2016-12-25', 7, 101, 2, 2),
(13, '2022-01-10', '2022-01-01', '2022-01-12', 1, 101, 1, 1),
(14, '2022-02-15', '2022-02-01', '2022-02-18', 2, 102, 2, 2),
(15, '2022-03-20', '2022-03-01', '2022-03-25', 3, 103, 3, 3),
(16, '2022-04-25', '2022-04-01', '2022-04-28', 4, 104, 4, 4);  -- Adicionando novas internações com um novo quarto

insert into procedimento (codigo_procedimento, nome_procedimento, descricao_procedimento) values
(1, 'Cirurgia Cardíaca', 'Cirurgia para correção de problemas cardíacos'),
(2, 'Exame de Sangue', 'Coleta e análise de amostras de sangue'),
(3, 'Raio-X', 'Imagem radiográfica para diagnóstico'),
(4, 'Endoscopia', 'Exame do trato gastrointestinal'),
(5, 'Quimioterapia', 'Tratamento para câncer usando medicamentos');

insert into procedimento_internacao (codigo_internacao, codigo_procedimento) values
(1, 1),
(1, 2),
(2, 3),
(2, 4),
(3, 5),
(4, 1),
(5, 2),
(6, 3),
(7, 4),
(13, 1),
(13, 2),
(14, 3),
(14, 4),
(15, 5),
(16, 1),
(16, 2);

insert into enfermeiro (codigo_enfermeiro, nome_enfermeiro, rg_enfermeiro, cpf_enfermeiro, cre_enfermeiro, data_nasc_enfermeiro) values
(1, 'Enf. João Costa', 'MG9876543', '33322211100', 'CRE12345/MG', '1985-01-01'),
(2, 'Enf. Maria Lima', 'SP8765432', '44433322211', 'CRE23456/SP', '1990-02-02'),
(3, 'Enf. Pedro Silva', 'RJ7654321', '55544433322', 'CRE34567/RJ', '1982-03-03'),
(4, 'Enf. Ana Santos', 'PR6543210', '66655544433', 'CRE45678/PR', '1975-04-04'),
(5, 'Enf. Carla Nunes', 'RS5432109', '77766655544', 'CRE56789/RS', '1983-05-05'),
(6, 'Enf. Bruno Souza', 'SC4321098', '88877766655', 'CRE67890/SC', '1991-06-06'),
(7, 'Enf. Paula Ribeiro', 'BA3210987', '99988877766', 'CRE78901/BA', '1974-07-07'),
(8, 'Enf. Luiz Martins', 'DF2109876', '11199988877', 'CRE89012/DF', '1987-08-08'),
(9, 'Enf. Fernanda Almeida', 'CE1098765', '22211199988', 'CRE90123/CE', '1976-09-09'),
(10, 'Enf. Ricardo Gonçalves', 'GO0987654', '33322211199', 'CRE01234/GO', '1992-10-10'),
(11, 'Enf. Mariana Silva', 'MG0987654', '44433322299', 'CRE12345/MG', '1995-11-11');  -- Adicionando um novo enfermeiro

insert into enfermeiro_internacao (codigo_enfermeiro, codigo_internacao) values
(1, 1),
(2, 1),
(3, 2),
(4, 2),
(5, 3),
(6, 3),
(7, 4),
(8, 4),
(9, 5),
(10, 5),
(1, 6),
(2, 6),
(3, 7),
(4, 7),
(11, 13),
(11, 14),
(11, 15),
(11, 16);  -- Associando o novo enfermeiro às internações existentes
```


## A Ordem do Alterar
- Um banco de dados pode sofrer alterações ao longo da sua concepção e do seu desenvolvimento. Nesse momento devemos nos preparar para atualizar nossas estratégias. Pensando no banco que já foi criado para o Projeto do Hospital, realize algumas alterações nas tabelas e nos dados usando comandos de atualização e exclusão: Crie um script que adicione uma coluna “em_atividade” para os médicos, indicando se ele ainda está atuando no hospital ou não. Crie um script para atualizar ao menos dois médicos como inativos e os demais em atividade.

```sql
alter table medico add column em_atividade varchar(10);

update medico
set em_atividade = "inativo"
where codigo_medico between 1 and 2;

update medico
set em_atividade = "ativo"
where codigo_medico between 3 and 10;

select * from medico;
```

## A Relíquias dos Dados
Uma vez que o banco estiver bem estruturado e desenhado, é possível realizar testes, simulando relatórios ou telas que o sistema possa necessitar. A tarefa consiste em criar consultas que levem aos resultados esperados.

- Todos os dados e o valor médio das consultas do ano de 2020 e das que foram feitas sob convênio.

 ```sql
  select * from consulta where year(data_consulta) = 2020 and numero_carteira_convenio is not null;
  ```

- Todos os dados das internações que tiveram data de alta maior que a data prevista para a alta.

```sql
select * from internacao where data_alta > data_prevista_alta;
```

- Receituário completo da primeira consulta registrada com receituário associado.

```sql
select * 
from receita as r
inner join consulta as c
on r.codigo_consulta = c.codigo_consulta
order by data_consulta asc
limit 1;
```

- Todos os dados da consulta de maior valor e também da de menor valor (ambas as consultas não foram realizadas sob convênio).

```sql
select * from consulta where valor_consulta = (select max(valor_consulta) from consulta) or valor_consulta = (select min(valor_consulta) from consulta);
```

- Todos os dados das internações em seus respectivos quartos, calculando o total da internação a partir do valor de diária do quarto e o número de dias entre a entrada e a alta.

```sql
select i.data_prevista_alta, i.data_entrada, i.data_alta, q.numero_quarto, tq.descricao_quarto, tq.diaria_quarto * datediff(i.data_alta, i.data_entrada) as total_internacao
from internacao as i
inner join quarto as q
on i.numero_quarto = q.numero_quarto
inner join tipo_quarto as tq
on q.codigo_tipo_quarto = tq.codigo_tipo_quarto;
```

- Data, procedimento e número de quarto de internações em quartos do tipo “apartamento”.

```sql
select i.data_entrada, p.descricao_procedimento, q.numero_quarto, tq.descricao_quarto
from internacao as i
inner join procedimento_internacao ip
on ip.codigo_internacao = i.codigo_internacao
inner join procedimento p
on ip.codigo_procedimento = p.codigo_procedimento
inner join quarto as q
on i.numero_quarto = q.numero_quarto
inner join tipo_quarto as tq
on tq.codigo_tipo_quarto = q.codigo_tipo_quarto
where tq.descricao_quarto = 'Apartamento';
```

- Data, procedimento e número de quarto de internações em quartos do tipo “apartamento”.

```sql
select i.data_entrada, p.descricao_procedimento, q.numero_quarto, tq.descricao_quarto
from internacao as i
inner join procedimento_internacao ip
on ip.codigo_internacao = i.codigo_internacao
inner join procedimento p
on ip.codigo_procedimento = p.codigo_procedimento
inner join quarto as q
on i.numero_quarto = q.numero_quarto
inner join tipo_quarto as tq
on tq.codigo_tipo_quarto = q.codigo_tipo_quarto
where tq.descricao_quarto = 'Apartamento';
```

- Nome do paciente, data da consulta e especialidade de todas as consultas em que os pacientes eram menores de 18 anos na data da consulta e cuja especialidade não seja “pediatria”, ordenando por data de realização da consulta.
- Nome do paciente, nome do médico, data da internação e procedimentos das internações realizadas por médicos da especialidade “gastroenterologia”, que tenham acontecido em “enfermaria”.
- Os nomes dos médicos, seus CRMs e a quantidade de consultas que cada um realizou.

```sql
select nome_medico, crm_medico, count(codigo_consulta) as numero_consultas
from medico as m
inner join consulta as c on m.codigo_medico = c.codigo_medico group by nome_medico, crm_medico;
```

- Todos os médicos que tenham "Gabriel" no nome.

```sql
select * from medico where nome_medico = 'Gabriel';
```

- Os nomes, CREs e número de internações de enfermeiros que participaram de mais de uma internação.

```sql
select nome_enfermeiro, cre_enfermeiro, count(i.codigo_internacao) as internacoes
from enfermeiro as e
inner join enfermeiro_internacao as ei
on e.codigo_enfermeiro = ei.codigo_enfermeiro
inner join internacao as i 
on i.codigo_internacao = ei.codigo_internacao
group by nome_enfermeiro, cre_enfermeiro;
```

