# Controle de Atendimentos SAT — V5: retornos e treinamento recente

## O que mudou

- **Registrar retorno**: dentro de Detalhes, abre um formulário para um novo contato pelo **mesmo problema**, com técnico, equipamento, status, motivo de não resolução, check, observações e treinamento recente. A data e hora do retorno são registradas automaticamente. O atendimento original não é sobrescrito.
- **Novo problema**: dentro de Detalhes, preenche os dados do cliente no formulário de novo atendimento para um caso independente.
- **Histórico cronológico**: contato inicial + retornos. É possível editar o resultado e as observações de cada retorno, preservando a data original daquele contato.
- **Treinamento recente (opcional)**: Menos de 1 semana; Menos de 3 semanas; Menos de 1 mês; Menos de 6 meses. Deixe em branco para treinamentos antigos, desconhecidos ou não aplicáveis. Cada contato pode ter sua própria faixa.
- **Dashboard**: dados separados de casos novos, contatos de retorno, casos com retorno registrado e casos resolvidos no primeiro contato sem retorno registrado até a exportação; resultados por técnico, motivos, distribuição por treinamento recente. O filtro por data considera a data **de cada contato**, inclusive retornos.
- **PDF**: 2 páginas A4, com resumo dos contatos e indicadores de retornos/treinamento.
- **Backups**: V5 inclui `records` e `returns`. Importa backups antigos V3/V4 (que só têm `records`).
- **Arquivar / Restaurar**: substitui a exclusão definitiva para não perder o histórico. A tabela pode alternar entre Ativos e Arquivados. Arquivados não contam no dashboard.

## Publicar sem perder os dados

1. Abra Firebase Console → **projeto `controle-atendimentos-sat`** → Firestore Database → Regras.
2. Copie as regras atuais para backup. Substitua pelo **`firestore.rules` deste pacote** e clique em **Publicar**. Isso libera a coleção `retornos` e os campos `trainingRecent` e `archived` no esquema do atendimento. **Não apague nem recrie a coleção `atendimentos`.**
3. Entre no **repositório GitHub do Controle de Atendimentos SAT** (não é o repositório de Pedidos). Substitua apenas `index.html` pelo arquivo V5 e confirme o commit na branch que publica o GitHub Pages.
4. Os arquivos `firebase-config.js` e `.nojekyll` podem permanecer como estão. Foram incluídos novamente neste pacote apenas para instalação nova.
5. Atualize a página publicada com Ctrl+F5. Teste primeiro usando um registro fictício.

## Como interpretar indicadores

- **Casos novos**: cada cadastro inicial (na data de abertura).
- **Contatos de retorno**: cada retorno vinculado àquele caso (na data de retorno).
- **Total de contatos**: casos iniciais + retornos dentro do filtro de datas.
- **Casos com retorno registrado**: dos casos **abertos no período**, quantos têm retorno registrado em qualquer data, inclusive posterior ao período.
- **Resolvidos no primeiro contato, sem retorno registrado**: dos casos **abertos no período**, aqueles cujo primeiro contato foi marcado como resolvido e que ainda não possuem retorno registrado. É um indicador observacional, não prova de resolução definitiva.
- **Treinamento recente**: faixas classificatórias selecionadas pelo técnico. Não inclua treinamentos antigos ou de idade desconhecida; deixe em branco. Faixas não são somadas entre si.

## Segurança — importante

O SAT continua **sem tela de login**, utilizando Firebase Authentication anônimo. Os dados NÃO são privados: qualquer pessoa com acesso ao link pode ler e alterar informações, e a autenticação anônima **não identifica quem realizou cada alteração**. Use dados fictícios durante testes; não coloque telefones, nomes ou dados reais de clientes antes de restringir o acesso.

Não foram feitas alterações no Firebase real ou no GitHub automaticamente: o pacote está pronto para publicação manual.
