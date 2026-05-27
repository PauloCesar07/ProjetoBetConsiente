USE ProjetoBetConsiente;
GO

SELECT 
    U.ID_Dispositivo,
    C.NomeCategoria AS TipoDeJogo,
    H.DataAcesso,
    H.HorarioAcesso,
    H.TempoPermanencido AS TempoSegundos,
    (H.TempoPermanencido / 60) AS TempoMinutos
FROM HistoricoComportamental H
INNER JOIN Usuarios U ON H.UsuarioID = U.UsuarioID
INNER JOIN CategoriaJogos C ON H.CategoriaID = C.CategoriaID;
GO

USE ProjetoBetConsiente;
GO

SELECT 
    U.ID_Dispositivo,
    Q.DataResposta,
    Q.PontuacaoTotal,
    Q.ClassificacaoRisco
FROM Questionarios Q
INNER JOIN Usuarios U ON Q.UsuarioID = U.UsuarioID
ORDER BY Q.PontuacaoTotal DESC;
GO

USE ProjetoBetConsiente;
GO

SELECT 
    U.ID_Dispositivo,
    A.DataAlerta,
    A.TipoAlerta,
    A.DirecionamentoRAPS
FROM Alertas A
INNER JOIN Usuarios U ON A.UsuarioID = U.UsuarioID;
GO