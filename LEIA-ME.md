# Controle de Atendimentos — online e SEM tela de login

Esta versão conserva a interface V3, o cadastro de clientes e técnicos, motivo obrigatório de atendimento não resolvido, check/observações dentro dos detalhes (duplo clique) e o dashboard. O GitHub Pages serve os arquivos HTML/JS; o Firebase Firestore armazena e sincroniza os registros.

## ⚠ Aviso de privacidade e segurança

**Sem login não significa acesso restrito.** O navegador usa a Autenticação Anônima do Firebase automaticamente (sem tela de login), mas **qualquer pessoa que encontre o link poderá visualizar, modificar e excluir os dados**. As regras aqui incluídas validam o formato dos registros, mas **não impedem que visitantes anônimos tenham acesso**. A configuração Web do Firebase é pública e não é uma senha. **Não coloque dados reais ou confidenciais de clientes nesta versão sem login.** Para uso de trabalho com dados pessoais, habilite depois controle de acesso por usuários autorizados (ou outra camada de autenticação segura).

## 1. Antes de migrar: backup da V3

No mesmo navegador e computador da versão offline V3, clique em **Backup** e salve o arquivo JSON. O novo endereço do GitHub Pages não lê automaticamente o localStorage da V3.

## 2. Configurar o Firebase

1. Abra https://console.firebase.google.com/ e crie **um projeto exclusivo** para esse painel. Separar projetos evita interferir nos outros sistemas.
2. Entre em **Visão geral do projeto > Configurações do projeto > Seus aplicativos > Adicionar app da Web**. O arquivo `firebase-config.js` **já foi preenchido** com os valores do projeto `controle-atendimentos-sat`. Não é necessário editar essa configuração.
3. Vá a **Authentication > Sign-in method** e **ative somente o provedor Anônimo** para este sistema. Não precisa adicionar usuários, e-mails, senhas ou UIDs. Atenção: isso não limita quem entra.
4. Vá a **Firestore Database > Criar banco de dados**, escolha uma região e o modo **Produção**.
5. Em **Firestore Database > Rules (Regras)**, substitua pelo conteúdo de `firestore.rules` e clique em **Publish**. Essas regras permitem acesso a visitantes anônimos somente à coleção `atendimentos`, com validação de formato. Não use `allow read, write: if true`.
6. Se aparecer erro de domínio autorizado, confira **Authentication > Settings > Authorized domains** e adicione o domínio do GitHub Pages (`JRSSantoss.github.io`, se mantiver essa conta).

## 3. Hospedar no GitHub Pages

1. Na conta GitHub, acesse https://github.com/new e crie um repositório novo, por exemplo `controle-atendimentos` (público para Pages no plano Free).
2. Envie para a raiz do repositório: **`index.html`, `firebase-config.js`, `.nojekyll`** (já configurados para `controle-atendimentos-sat`). Você pode enviar também `LEIA-ME.md`; o arquivo `firestore.rules` deve ser aplicado no console do Firebase (é útil mantê-lo como referência no repositório).
3. Em **Settings > Pages**, selecione **Deploy from a branch > main > / (root)** e salve.
4. O endereço terá formato `https://SEU-USUARIO.github.io/controle-atendimentos/` depois de publicar. A página inicia sem pedir login; acessa automaticamente o Firebase.
5. Faça um teste com dados fictícios, abra em outro navegador e confirme que os registros e o dashboard se sincronizam.

## 4. Importar backup antigo

1. Na V3, exporte o backup JSON.
2. No site online, clique em **Importar backup** e selecione esse JSON.
3. O processo adiciona apenas IDs ainda inexistentes, sem apagar registros já na nuvem.
4. Confira os totais e o dashboard e mantenha cópias periódicas.

## Funcionalidades

- Cadastro rápido: nome/contato, empresa, telefone, técnico (João ou Ricardo).
- Tabela concisa: data, cliente, empresa, telefone, técnico, equipamento, conclusão e ações.
- Duplo clique na linha ou botão **Detalhes**: observações, check e motivo obrigatório de **Não resolvido**.
- Gráficos e indicadores: total, resolvidos, não resolvidos, pendentes, motivos e resultados por técnico.
- Filtros, exportação CSV, backup JSON, importação e sincronização em tempo real entre aparelhos.
- **Sem tela de login**. Internet necessária para alterações confirmadas.

## Diagnóstico

- **Tela de configuração**: verifique `firebase-config.js` e a publicação no GitHub.
- **Não foi possível conectar**: confira se ativou **Authentication > Anônimo** e a conexão à internet.
- **Acesso negado**: confira o projeto, as regras publicadas e se o provedor Anônimo está ligado.
- **Site abre mas registros não aparecem**: veja se o JSON da V3 foi importado e se os arquivos apontam para o mesmo projeto Firebase.

Documentação oficial: https://firebase.google.com/docs/auth/web/anonymous-auth ; https://firebase.google.com/docs/rules/insecure-rules ; https://firebase.google.com/docs/firestore/query-data/listen ; https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
