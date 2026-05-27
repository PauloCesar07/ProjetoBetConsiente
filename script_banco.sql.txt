CREATE DATABASE ProjetoBetConsiente;
GO

USE ProjetoBetConsiente;
GO

-- ==========================================
-- TABELA DOS USUÁRIOS
-- ==========================================
CREATE TABLE Usuarios (
     UsuarioID INT IDENTITY (1,1) PRIMARY KEY,
     ID_Dispositivo VARCHAR(100) NOT NULL UNIQUE,
     DataCadastro DATETIME DEFAULT GETDATE(),
     StatusUsuario VARCHAR(20) DEFAULT 'Ativo'
);
GO

-- ==========================================
-- TABELA DO HISTÓRICO COMPORTAMENTAL
-- ==========================================

-- ---------------------------------------------
-- TABELA CATEGORIA DOS JOGOS
-- ---------------------------------------------
CREATE TABLE CategoriaJogos (
      CategoriaID INT IDENTITY (1,1) PRIMARY KEY,
      NomeCategoria VARCHAR(100) NOT NULL UNIQUE 
      );

INSERT INTO CategoriaJogos (NomeCategoria) VALUES
('Slots / Jogos Cash'),
('Crash '),
('Aposta Esportivas'),
('Cassino AO VIVO') ;
GO


      
CREATE TABLE HistoricoComportamental (
      RegistroID BIGINT IDENTITY (1,1) PRIMARY KEY,
      UsuarioID INT NOT NULL,
      DataAcesso DATE NOT NULL, 
      CategoriaID INT NOT NULL,
      HorarioAcesso TIME NOT NULL,
      TempoPermanencido INT NOT NULL, -- Quanto tempo o usuário ficou no app de jogo
      CONSTRAINT FK_Historico_Usuarios FOREIGN KEY (UsuarioID) REFERENCES Usuarios(UsuarioID),
      CONSTRAINT FK_Historico_Categorias FOREIGN KEY (CategoriaID) REFERENCES CategoriaJogos(CategoriaID)
);
GO

CREATE NONCLUSTERED INDEX IX_HistoricoComportamental_UsuarioID ON HistoricoComportamental(UsuarioID);
CREATE NONCLUSTERED INDEX IX_HistoricoComportamental_CategoriaID ON HistoricoComportamental(CategoriaID);
GO

-- ==========================================
-- TABELA DO QUESTIONÁRIO (PGSI)
-- ==========================================
CREATE TABLE Questionarios (
      AvaliacaoID INT IDENTITY (1,1) PRIMARY KEY,
      UsuarioID INT NOT NULL,
      DataResposta DATETIME DEFAULT GETDATE(),
      PontuacaoTotal INT NOT NULL,
      ClassificacaoRisco VARCHAR(50) NOT NULL,
      CONSTRAINT FK_Avaliacoes_Usuarios FOREIGN KEY (UsuarioID) REFERENCES Usuarios(UsuarioID)
);
GO
      CREATE NONCLUSTERED INDEX IX_Questionarios_UsuarioID ON Questionarios(UsuarioID);
GO

-- ==========================================
-- TABELA DE ALERTAS E ENCAMINHAMENTOS
-- ==========================================
CREATE TABLE Alertas (
      AlertaID INT IDENTITY (1,1) PRIMARY KEY,
      UsuarioID INT NOT NULL,
      DataAlerta DATETIME DEFAULT GETDATE(),
      TipoAlerta VARCHAR(50) NOT NULL,
      DirecionamentoRAPS VARCHAR(100) NOT NULL,
      CONSTRAINT FK_Alertas_Usuarios FOREIGN KEY (UsuarioID) REFERENCES Usuarios(UsuarioID)
);
GO

     CREATE NONCLUSTERED INDEX IX_Alertas_UsuarioID ON Alertas(UsuarioID);
GO


----------------------------------------------------------------------------------

-- ==========================================
-- 1. INSERINDO USUÁRIOS DE TESTE
-- ==========================================
INSERT INTO Usuarios (ID_Dispositivo, StatusUsuario) 
VALUES 
('android_abc123789xyz', 'Ativo'),
('ios_987654321lkj', 'Ativo');
GO

-- Como o ID do usuário é gerado automaticamente (IDENTITY), 
-- vamos buscar os IDs dinamicamente para os próximos inserts
DECLARE @Usuario1 INT, @Usuario2 INT;
SELECT @Usuario1 = UsuarioID FROM Usuarios WHERE ID_Dispositivo = 'android_abc123789xyz';
SELECT @Usuario2 = UsuarioID FROM Usuarios WHERE ID_Dispositivo = 'ios_987654321lkj';

-- ==========================================
-- 2. INSERINDO HISTÓRICO COMPORTAMENTAL
-- ==========================================
INSERT INTO HistoricoComportamental (UsuarioID, CategoriaID, DataAcesso, HorarioAcesso, TempoPermanencido)
VALUES 
(@Usuario1, 1, '2026-05-25', '14:30:00', 600), -- Usuário 1 em Slots
(@Usuario1, 2, '2026-05-25', '14:42:00', 300), -- Usuário 1 em Crash
(@Usuario2, 3, '2026-05-26', '20:15:00', 900); -- Usuário 2 em Apostas Esportivas
GO

-- ==========================================
-- 3. INSERINDO RESPOSTAS DE QUESTIONÁRIOS (PGSI)
-- ==========================================
DECLARE @Usuario1 INT;
SELECT @Usuario1 = UsuarioID FROM Usuarios WHERE ID_Dispositivo = 'android_abc123789xyz';
DECLARE @Usuario2 INT;
SELECT @Usuario2 = UsuarioID FROM Usuarios WHERE ID_Dispositivo = 'ios_987654321lkj';

INSERT INTO Questionarios (UsuarioID, DataResposta, PontuacaoTotal, ClassificacaoRisco)
VALUES 
(@Usuario1, GETDATE(), 9, 'Jogo Problemático (Alto Risco)'),
(@Usuario2, GETDATE(), 2, 'Baixo Risco');
GO

-- ==========================================
-- 4. INSERINDO ALERTAS
-- ==========================================
DECLARE @Usuario1 INT;
SELECT @Usuario1 = UsuarioID FROM Usuarios WHERE ID_Dispositivo = 'android_abc123789xyz';

INSERT INTO Alertas (UsuarioID, DataAlerta, TipoAlerta, DirecionamentoRAPS)
VALUES 
(@Usuario1, GETDATE(), 'Notificação de Alto Risco - PGSI', 'Encaminhado para o CAPS AD da região do usuário');
GO