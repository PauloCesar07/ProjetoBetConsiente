# ProjetoBetConsiente
# Projeto Bet Consciente - Banco de Dados 🧠🛡️

Este repositório contém a estrutura de modelagem de dados para o **Projeto Bet Consciente**, uma iniciativa voltada para o monitoramento comportamental, conscientização e apoio à saúde mental em relação a jogos de azar e apostas online.

O banco de dados foi desenvolvido em **SQL Server (T-SQL)**.

## 📊 Estrutura do Banco de Dados

O modelo é composto por 5 tabelas principais, otimizadas com índices de performance para buscas rápidas:

1. **Usuarios:** Identifica os dispositivos dos usuários de forma única (`ID_Dispositivo`).
2. **CategoriaJogos:** Armazena os tipos de plataformas monitoradas (Slots, Crash Games, Apostas Esportivas, Cassino Ao Vivo).
3. **HistoricoComportamental:** Registra o tempo de permanência (em segundos) do usuário por categoria de jogo.
4. **Questionarios:** Armazena os resultados do teste PGSI (Problem Gambling Severity Index) e a classificação de risco.
5. **Alertas:** Registra os gatilhos de alto risco e o direcionamento automático para a RAPS (Rede de Atenção Psicossocial / CAPS).

## 🚀 Como Executar

1. Abra o **SQL Server Management Studio (SSMS)** ou sua IDE de preferência.
2. Abra e execute o arquivo `script_banco.sql` para criar o banco de dados, as tabelas e os dados de teste iniciais.
3. Certifique-se de que a conexão no seu projeto Java (ex: `application.properties` do Spring) esteja apontando para o banco `ProjetoBetConsiente`.

## 🛠️ Informações para o Desenvolvedor Java
- A chave primária da tabela `HistoricoComportamental` é `BIGINT`, pois acumulará muitos registros de logs.
- O campo `TempoPermanencido` armazena inteiros que representam o tempo em **segundos**.
- Os relacionamentos estão validados por chaves estrangeiras (`FOREIGN KEY`), garantindo a integridade dos dados ao inserir registros do backend.
