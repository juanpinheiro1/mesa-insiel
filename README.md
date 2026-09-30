# Mesa de Comando Insiel

Painel interno da Insiel para gerir o que o site insiel.com.br gera: pedidos de orçamento, chamados técnicos e contatos.
Página única (`index.html`) publicada no GitHub Pages (repo `mesa-insiel`), banco no Supabase (projeto ABRAPE SOLAR `puepmnyffpfjttxmwfjt`, schema `insiel`).

- Login: Supabase Auth (e-mail + senha). Só e-mails na tabela `insiel.usuarios` (ativos) veem dados (RLS via `insiel.autorizado()`; admin via `insiel.admin()`).
- Tabelas: `leads` (site grava via RPC pública), `lead_eventos` (histórico/observações; trigger registra mudanças de status, responsável e visita), `usuarios`, view `v_resumo`.
- Requisito de configuração no painel Supabase: **Project Settings → Data API → Exposed schemas → adicionar `insiel`** e **Authentication → URL Configuration → Site URL / Redirect URLs** com o endereço da Mesa.
- Preview local: launch.json `mesa-insiel` (porta 8324). Deploy: `git push`.
