# projetoSQL

-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS mydb DEFAULT CHARACTER SET utf8 ;
USE mydb ;

-- -----------------------------------------------------
-- Table mydb.Transacao
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.Transacao (
  ID_Trasacao INT NOT NULL AUTO_INCREMENT,
  Valor DECIMAL(9,2) NOT NULL,
  MetodoPagamento VARCHAR(45) NOT NULL,
  Data DATE NOT NULL,
  PRIMARY KEY (ID_Trasacao))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.Profissional
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.Profissional (
  ID_Profissional INT NOT NULL AUTO_INCREMENT,
  Nome VARCHAR(45) NOT NULL,
  DataNascimento DATE NOT NULL,
  Telefone VARCHAR(45) NOT NULL,
  Profissao VARCHAR(45) NOT NULL,
  PRIMARY KEY (ID_Profissional))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.EmpresaCliente
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.EmpresaCliente (
  CNPJ VARCHAR(14) NOT NULL,
  Email VARCHAR(45) NOT NULL,
  Telefone VARCHAR(45) NOT NULL,
  NomeFantasia VARCHAR(80) NOT NULL,
  RepresentanteLegal VARCHAR(45) NOT NULL,
  PRIMARY KEY (CNPJ))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.Contrato
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.Contrato (
  ID_Contrato INT NOT NULL,
  DataInicio DATE NOT NULL,
  DataTermino DATE NOT NULL,
  ValorPago DECIMAL(7,2) NOT NULL,
  Status ENUM("Ativo", "Concluido", "Cancelado") NOT NULL DEFAULT 'Ativo',
  Profissional_ID INT NOT NULL,
  EmpresaCliente_CNPJ VARCHAR(14) NOT NULL,
  PRIMARY KEY (ID_Contrato, Profissional_ID, EmpresaCliente_CNPJ),
  INDEX fk_Contrato_Profissional1_idx (Profissional_ID ASC) VISIBLE,
  INDEX fk_Contrato_EmpresaCliente1_idx (EmpresaCliente_CNPJ ASC) VISIBLE,
  CONSTRAINT fk_Contrato_Profissional1
    FOREIGN KEY (Profissional_ID)
    REFERENCES mydb.Profissional (ID_Profissional)
    ON DELETE RESTRICT
    ON UPDATE CASCADE,
  CONSTRAINT fk_Contrato_EmpresaCliente1
    FOREIGN KEY (EmpresaCliente_CNPJ)
    REFERENCES mydb.EmpresaCliente (CNPJ)
    ON DELETE RESTRICT
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.FeedbackAvaliacao
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.FeedbackAvaliacao (
  ID_FeedbackAvaliacao INT(20) NOT NULL AUTO_INCREMENT,
  Data DATE NOT NULL,
  Comentario MEDIUMTEXT NULL,
  Classificacao MEDIUMTEXT NULL,
  Contrato_ID INT NOT NULL,
  Contrato_Profissional_ID INT NOT NULL,
  Contrato_EmpresaCliente_CNPJ VARCHAR(14) NOT NULL,
  PRIMARY KEY (ID_FeedbackAvaliacao, Contrato_ID, Contrato_Profissional_ID, Contrato_EmpresaCliente_CNPJ),
  INDEX fk_FeedbackAvaliacao_Contrato1_idx (Contrato_ID ASC, Contrato_Profissional_ID ASC, Contrato_EmpresaCliente_CNPJ ASC) VISIBLE,
  CONSTRAINT fk_FeedbackAvaliacao_Contrato1
    FOREIGN KEY (Contrato_ID , Contrato_Profissional_ID , Contrato_EmpresaCliente_CNPJ)
    REFERENCES mydb.Contrato (ID_Contrato , Profissional_ID , EmpresaCliente_CNPJ)
    ON DELETE RESTRICT
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.VagaDeEmprego
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.VagaDeEmprego (
  ID_VagaDeEmprego INT NOT NULL AUTO_INCREMENT,
  Cargo VARCHAR(60) NOT NULL,
  Funcao VARCHAR(45) NOT NULL,
  Local VARCHAR(200) NOT NULL,
  QntVagas INT NOT NULL,
  EmpresaCliente_CNPJ VARCHAR(14) NOT NULL,
  PRIMARY KEY (ID_VagaDeEmprego, EmpresaCliente_CNPJ),
  INDEX fk_VagaDeEmprego_EmpresaCliente1_idx (EmpresaCliente_CNPJ ASC) VISIBLE,
  CONSTRAINT fk_VagaDeEmprego_EmpresaCliente1
    FOREIGN KEY (EmpresaCliente_CNPJ)
    REFERENCES mydb.EmpresaCliente (CNPJ)
    ON DELETE RESTRICT
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.Habilidade
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.Habilidade (
  ID_Habilidade INT NOT NULL AUTO_INCREMENT,
  Habilidade MEDIUMTEXT NOT NULL,
  Profissional_ID INT NOT NULL,
  PRIMARY KEY (ID_Habilidade, Profissional_ID),
  INDEX fk_Habilidade_Profissional1_idx (Profissional_ID ASC) VISIBLE,
  CONSTRAINT fk_Habilidade_Profissional1
    FOREIGN KEY (Profissional_ID)
    REFERENCES mydb.Profissional (ID_Profissional)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.AgendamentoEntrevistas
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.AgendamentoEntrevistas (
  ID_Agendamento INT NOT NULL,
  Data DATETIME NOT NULL,
  Detalhes MEDIUMTEXT NOT NULL,
  Profissional_ID INT NOT NULL,
  EmpresaCliente_CNPJ VARCHAR(14) NOT NULL,
  PRIMARY KEY (ID_Agendamento, Profissional_ID, EmpresaCliente_CNPJ),
  INDEX fk_AgendamentoEntrevistas_Profissional1_idx (Profissional_ID ASC) VISIBLE,
  INDEX fk_AgendamentoEntrevistas_EmpresaCliente1_idx (EmpresaCliente_CNPJ ASC) VISIBLE,
  CONSTRAINT fk_AgendamentoEntrevistas_Profissional1
    FOREIGN KEY (Profissional_ID)
    REFERENCES mydb.Profissional (ID_Profissional)
    ON DELETE RESTRICT
    ON UPDATE CASCADE,
  CONSTRAINT fk_AgendamentoEntrevistas_EmpresaCliente1
    FOREIGN KEY (EmpresaCliente_CNPJ)
    REFERENCES mydb.EmpresaCliente (CNPJ)
    ON DELETE RESTRICT
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.Entrevistas
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.Entrevistas (
  ID_Entrevistas INT NOT NULL,
  Data DATE NOT NULL,
  Feedback MEDIUMTEXT NOT NULL,
  Notas DECIMAL(4,2) NOT NULL,
  Avaliacao MEDIUMTEXT NOT NULL,
  Profissional_ID INT NOT NULL,
  VagaDeEmprego_ID INT NOT NULL,
  EmpresaCliente_CNPJ VARCHAR(14) NOT NULL,
  PRIMARY KEY (ID_Entrevistas, VagaDeEmprego_ID, EmpresaCliente_CNPJ, Profissional_ID),
  INDEX fk_Entrevistas_Profissional1_idx (Profissional_ID ASC) VISIBLE,
  INDEX fk_Entrevistas_VagaDeEmprego1_idx (VagaDeEmprego_ID ASC, EmpresaCliente_CNPJ ASC) VISIBLE,
  CONSTRAINT fk_Entrevistas_Profissional1
    FOREIGN KEY (Profissional_ID)
    REFERENCES mydb.Profissional (ID_Profissional)
    ON DELETE RESTRICT
    ON UPDATE CASCADE,
  CONSTRAINT fk_Entrevistas_VagaDeEmprego1
    FOREIGN KEY (VagaDeEmprego_ID , EmpresaCliente_CNPJ)
    REFERENCES mydb.VagaDeEmprego (ID_VagaDeEmprego , EmpresaCliente_CNPJ)
    ON DELETE RESTRICT
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.Endereço
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.Endereço (
  ID_Endereco INT NOT NULL,
  Tipo_Endereco VARCHAR(45) NOT NULL,
  Logradouro VARCHAR(45) NOT NULL,
  ID_TipoEndereco INT NULL,
  PRIMARY KEY (ID_Endereco))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.Pagamento
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.Pagamento (
  Transacao_ID INT NOT NULL,
  Profissional_ID INT NOT NULL,
  PRIMARY KEY (Transacao_ID, Profissional_ID),
  INDEX fk_Pagamento_Profissional1_idx (Profissional_ID ASC) VISIBLE,
  CONSTRAINT fk_Pagamento_Transacao1
    FOREIGN KEY (Transacao_ID)
    REFERENCES mydb.Transacao (ID_Trasacao)
    ON DELETE RESTRICT
    ON UPDATE CASCADE,
  CONSTRAINT fk_Pagamento_Profissional1
    FOREIGN KEY (Profissional_ID)
    REFERENCES mydb.Profissional (ID_Profissional)
    ON DELETE RESTRICT
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.Faturamento
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.Faturamento (
  Transacao_ID INT NOT NULL,
  EmpresaCliente_CNPJ VARCHAR(14) NOT NULL,
  PRIMARY KEY (Transacao_ID, EmpresaCliente_CNPJ),
  INDEX fk_Faturamento_EmpresaCliente1_idx (EmpresaCliente_CNPJ ASC) VISIBLE,
  CONSTRAINT fk_Faturamento_Transacao1
    FOREIGN KEY (Transacao_ID)
    REFERENCES mydb.Transacao (ID_Trasacao)
    ON DELETE RESTRICT
    ON UPDATE CASCADE,
  CONSTRAINT fk_Faturamento_EmpresaCliente1
    FOREIGN KEY (EmpresaCliente_CNPJ)
    REFERENCES mydb.EmpresaCliente (CNPJ)
    ON DELETE RESTRICT
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.EnderecoProfissional
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.EnderecoProfissional (
  Endereço_ID INT NOT NULL,
  Profissional_ID INT NOT NULL,
  PRIMARY KEY (Endereço_ID, Profissional_ID),
  INDEX fk_EnderecoProfissional_Profissional1_idx (Profissional_ID ASC) VISIBLE,
  CONSTRAINT fk_EnderecoProfissional_Endereço1
    FOREIGN KEY (Endereço_ID)
    REFERENCES mydb.Endereço (ID_Endereco)
    ON DELETE RESTRICT
    ON UPDATE CASCADE,
  CONSTRAINT fk_EnderecoProfissional_Profissional1
    FOREIGN KEY (Profissional_ID)
    REFERENCES mydb.Profissional (ID_Profissional)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.EnderecoEmpresa
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.EnderecoEmpresa (
  Endereco_ID INT NOT NULL,
  EmpresaCliente_CNPJ VARCHAR(14) NOT NULL,
  PRIMARY KEY (Endereco_ID, EmpresaCliente_CNPJ),
  INDEX fk_EnderecoEmpresa_EmpresaCliente1_idx (EmpresaCliente_CNPJ ASC) VISIBLE,
  CONSTRAINT fk_EnderecoEmpresa_Endereço1
    FOREIGN KEY (Endereco_ID)
    REFERENCES mydb.Endereço (ID_Endereco)
    ON DELETE RESTRICT
    ON UPDATE CASCADE,
  CONSTRAINT fk_EnderecoEmpresa_EmpresaCliente1
    FOREIGN KEY (EmpresaCliente_CNPJ)
    REFERENCES mydb.EmpresaCliente (CNPJ)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table mydb.EnderecoAgendamento
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mydb.EnderecoAgendamento (
  Endereço_ID INT NOT NULL,
  AgendamentoEntrevistas_ID INT NOT NULL,
  PRIMARY KEY (Endereço_ID, AgendamentoEntrevistas_ID),
  INDEX fk_EnderecoAgendamento_AgendamentoEntrevistas1_idx (AgendamentoEntrevistas_ID ASC) VISIBLE,
  CONSTRAINT fk_EnderecoAgendamento_Endereço1
    FOREIGN KEY (Endereço_ID)
    REFERENCES mydb.Endereço (ID_Endereco)
    ON DELETE RESTRICT
    ON UPDATE CASCADE,
  CONSTRAINT fk_EnderecoAgendamento_AgendamentoEntrevistas1
    FOREIGN KEY (AgendamentoEntrevistas_ID)
    REFERENCES mydb.AgendamentoEntrevistas (ID_Agendamento)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

INSERT INTO `mydb`.`Profissional` (`Nome`, `DataNascimento`, `Telefone`, `Profissao`) VALUES
('Ana Silva', '1985-03-12', '11987654321', 'Desenvolvedor'),
('Carlos Souza', '1990-07-25', '21987654321', 'Gerente de Projetos'),
('Juliana Martins', '1983-01-16', '31987654321', 'Analista de Sistemas'),
('Ricardo Oliveira', '1978-04-18', '41987654321', 'Consultor'),
('Mariana Costa', '1992-05-22', '51987654321', 'Designer'),
('Pedro Alvares', '1987-08-30', '61987654321', 'Engenheiro de Software'),
('Fernanda Correia', '1989-11-10', '71987654321', 'Product Manager'),
('Lucas Mendes', '1995-12-15', '81987654321', 'Analista de Dados'),
('Patricia Nobrega', '1980-09-05', '91987654321', 'Arquiteto de TI'),
('Eduardo Paz', '1975-02-23', '21987654322', 'CEO');

INSERT INTO `mydb`.`EmpresaCliente` (`CNPJ`, `Email`, `Telefone`, `NomeFantasia`,
`RepresentanteLegal`) VALUES
('12345678901234', 'contato@empresa1.com.br', '1133224455', 'Empresa1', 'Carlos Souza'),
('23456789012345', 'contato@empresa2.com.br', '2133224455', 'Empresa2', 'Juliana Martins'),
('34567890123456', 'contato@empresa3.com.br', '3133224455', 'Empresa3', 'Ana Silva'),
('45678901234567', 'contato@empresa4.com.br', '4133224455', 'Empresa4', 'Ricardo Oliveira'),
('56789012345678', 'contato@empresa5.com.br', '5133224455', 'Empresa5', 'Mariana Costa'),
('67890123456789', 'contato@empresa6.com.br', '6133224455', 'Empresa6', 'Pedro Alvares'),
('78901234567890', 'contato@empresa7.com.br', '7133224455', 'Empresa7', 'Fernanda Correia'),
('89012345678901', 'contato@empresa8.com.br', '8133224455', 'Empresa8', 'Lucas Mendes'),
('90123456789012', 'contato@empresa9.com.br', '9133224455', 'Empresa9', 'Patricia Nobrega'),
('01234567890123', 'contato@empresa10.com.br', '2133224456', 'Empresa10', 'Eduardo Paz');

INSERT INTO `mydb`.`Transacao` (`Valor`, `MetodoPagamento`, `Data`) VALUES
(1000.00, 'Cartão de Crédito', '2024-07-01'),
(1500.00, 'Boleto', '2024-07-02'),
(750.00, 'Pix', '2024-07-03'),
(1250.00, 'Transferência Bancária', '2024-07-04'),
(500.00, 'Cartão de Débito', '2024-07-05'),
(1750.00, 'Cartão de Crédito', '2024-07-06'),
(2000.00, 'Boleto', '2024-07-07'),
(800.00, 'Pix', '2024-07-08'),
(1450.00, 'Transferência Bancária', '2024-07-09'),
(650.00, 'Cartão de Débito', '2024-07-10');

INSERT INTO `mydb`.`Contrato` (`ID_Contrato`, `DataInicio`, `DataTermino`, `ValorPago`, `Status`,
`Profissional_ID`, `EmpresaCliente_CNPJ`) VALUES
(1, '2024-07-01', '2025-07-01', 12000.00, 'Ativo', 1, '12345678901234'),
(2, '2024-08-01', '2025-08-01', 30000.00, 'Ativo', 2, '23456789012345'),
(3, '2024-09-01', '2025-09-01', 20000.00, 'Concluido', 3, '34567890123456'),
(4, '2024-10-01', '2025-10-01', 50000.00, 'Cancelado', 4, '45678901234567'),
(5, '2024-11-01', '2025-11-01', 35000.00, 'Ativo', 5, '56789012345678'),
(6, '2024-12-01', '2025-12-01', 25000.00, 'Ativo', 6, '67890123456789'),
(7, '2025-01-01', '2026-01-01', 15000.00, 'Ativo', 7, '78901234567890'),
(8, '2025-02-01', '2026-02-01', 45000.00, 'Ativo', 8, '89012345678901'),
(9, '2025-03-01', '2026-03-01', 30000.00, 'Ativo', 9, '90123456789012'),
(10, '2025-04-01', '2026-04-01', 40000.00, 'Ativo', 10, '01234567890123');

INSERT INTO `mydb`.`FeedbackAvaliacao` (`Data`, `Comentario`, `Classificacao`, `Contrato_ID`,
`Contrato_Profissional_ID`, `Contrato_EmpresaCliente_CNPJ`) VALUES
('2024-07-15', 'Excelente serviço.', '5 estrelas', 1, 1, '12345678901234'),
('2024-07-16', 'Bom, mas pode melhorar.', '4 estrelas', 2, 2, '23456789012345'),
('2024-07-17', 'Não atendeu às expectativas.', '2 estrelas', 3, 3, '34567890123456'),
('2024-07-18', 'Ótimo atendimento.', '5 estrelas', 4, 4, '45678901234567'),
('2024-07-19', 'Muito bom!', '4 estrelas', 5, 5, '56789012345678'),
('2024-07-20', 'Serviço excelente.', '5 estrelas', 6, 6, '67890123456789'),
('2024-07-21', 'Regular, precisa de ajustes.', '3 estrelas', 7, 7, '78901234567890'),
('2024-07-22', 'Perfeito, muito bem feito!', '5 estrelas', 8, 8, '89012345678901'),
('2024-07-23', 'Aceitável, mas com atrasos.', '3 estrelas', 9, 9, '90123456789012'),
('2024-07-24', 'Excelente qualidade e entrega no prazo.', '5 estrelas', 10, 10, '01234567890123');

INSERT INTO `mydb`.`VagaDeEmprego` (`Cargo`, `Funcao`, `Local`, `QntVagas`,
`EmpresaCliente_CNPJ`) VALUES
('Desenvolvedor Web', 'Desenvolver sites e sistemas web', 'São Paulo', 3, '12345678901234'),
('Analista de Sistemas', 'Analisar e desenvolver sistemas', 'Rio de Janeiro', 2, '23456789012345'),
('Gerente de Projetos', 'Gerenciar projetos de TI', 'Belo Horizonte', 1, '34567890123456'),
('Designer Gráfico', 'Criar e revisar designs gráficos', 'Porto Alegre', 2, '45678901234567'),
('Engenheiro de Dados', 'Gerenciar e analisar grandes volumes de dados', 'Brasília', 1,
'56789012345678'),
('Product Manager', 'Gerenciar ciclos de vida de produtos', 'Curitiba', 1, '67890123456789'),
('CEO', 'Gestão executiva da empresa', 'Fortaleza', 1, '78901234567890'),
('Arquiteto de TI', 'Desenhar a estrutura de TI das organizações', 'Recife', 1, '89012345678901'),
('Consultor de TI', 'Consultoria em tecnologia da informação', 'Manaus', 1, '90123456789012'),
('Analista de Dados', 'Análise de dados para insights empresariais', 'Salvador', 2, '01234567890123');

INSERT INTO `mydb`.`Habilidade` (`Habilidade`, `Profissional_ID`) VALUES
('Java, Spring Boot', 1),
('Gerência de projetos, Scrum', 2),
('Análise de requisitos, UML', 3),
('Consultoria empresarial, Estratégia de TI', 4),
('Adobe Photoshop, Adobe Illustrator', 5),
('Python, Análise de dados', 6),
('Gestão de produtos, Roadmapping', 7),
('SQL, NoSQL, Data warehousing', 8),
('Arquitetura de sistemas, Microservices', 9),
('Liderança, Gestão de equipes', 10);

INSERT INTO `mydb`.`AgendamentoEntrevistas` (`ID_Agendamento`, `Data`, `Detalhes`,
`Profissional_ID`, `EmpresaCliente_CNPJ`) VALUES
(1, '2024-08-01 14:00:00', 'Entrevista para posição de Desenvolvedor Web', 1, '12345678901234'),
(2, '2024-08-02 15:00:00', 'Entrevista para posição de Analista de Sistemas', 3, '23456789012345'),
(3, '2024-08-03 10:00:00', 'Reunião para discussão de projeto', 2, '34567890123456'),
(4, '2024-08-04 09:00:00', 'Entrevista para Designer Gráfico', 5, '45678901234567'),
(5, '2024-08-05 16:00:00', 'Discussão de contrato de Engenharia de Dados', 6, '56789012345678'),
(6, '2024-08-06 11:00:00', 'Entrevista para Product Manager', 7, '67890123456789'),
(7, '2024-08-07 13:00:00', 'Entrevista para CEO', 10, '78901234567890'),
(8, '2024-08-08 12:00:00', 'Revisão de projeto de TI com Arquiteto', 9, '89012345678901'),
(9, '2024-08-09 17:00:00', 'Consultoria em TI', 4, '90123456789012'),
(10, '2024-08-10 10:30:00', 'Entrevista para Analista de Dados', 8, '01234567890123');

INSERT INTO `mydb`.`Entrevistas` (`ID_Entrevistas`, `Data`, `Feedback`, `Notas`, `Avaliacao`,
`Profissional_ID`, `VagaDeEmprego_ID`, `EmpresaCliente_CNPJ`) VALUES
(1, '2024-08-01', 'Muito bem preparado e comunicativo.', 4.5, 'Aprovado', 1, 1, '12345678901234'),
(2, '2024-08-02', 'Precisa melhorar nas habilidades técnicas.', 3.0, 'Em espera', 3, 2,
'23456789012345'),
(3, '2024-08-03', 'Excelente liderança, falta conhecimento técnico.', 3.5, 'Em espera', 2, 3,
'34567890123456'),
(4, '2024-08-04', 'Criativo e proativo.', 4.8, 'Aprovado', 5, 4, '45678901234567'),
(5, '2024-08-05', 'Dados analisados com precisão.', 4.9, 'Aprovado', 6, 5, '56789012345678'),
(6, '2024-08-06', 'Bom, mas carece de experiência em gestão.', 3.7, 'Em espera', 7, 6,
'67890123456789'),
(7, '2024-08-07', 'Experiência excepcional, alto custo.', 4.6, 'Aprovado', 10, 7, '78901234567890'),
(8, '2024-08-08', 'Requer mais experiência em grandes projetos.', 3.2, 'Em espera', 9, 8,
'89012345678901'),
(9, '2024-08-09', 'Muito bom consultor, recomendado.', 4.7, 'Aprovado', 4, 9, '90123456789012'),
(10, '2024-08-10', 'Ótimo em análise, falta comunicação.', 3.9, 'Em espera', 8, 10, '01234567890123');

INSERT INTO `mydb`.`Pagamento` (`Transacao_ID`, `Profissional_ID`) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);

INSERT INTO `mydb`.`Faturamento` (`Transacao_ID`, `EmpresaCliente_CNPJ`) VALUES
(1, '12345678901234'),
(2, '23456789012345'),
(3, '34567890123456'),
(4, '45678901234567'),
(5, '56789012345678'),
(6, '67890123456789'),
(7, '78901234567890'),
(8, '89012345678901'),
(9, '90123456789012'),
(10, '01234567890123');

INSERT INTO `mydb`.`EnderecoProfissional` (`Endereço_ID`, `Profissional_ID`) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);
INSERT INTO `mydb`.`EnderecoEmpresa` (`Endereco_ID`, `EmpresaCliente_CNPJ`) VALUES
(1, '12345678901234'),
(2, '23456789012345'),
(3, '34567890123456'),
(4, '45678901234567'),
(5, '56789012345678'),
(6, '67890123456789'),
(7, '78901234567890'),
(8, '89012345678901'),
(9, '90123456789012'),
(10, '01234567890123');

INSERT INTO `mydb`.`EnderecoAgendamento` (`Endereço_ID`, `AgendamentoEntrevistas_ID`)
VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);
