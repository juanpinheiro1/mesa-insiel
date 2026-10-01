# Mesa de Comando Insiel

Painel interno da Insiel para gerir o que o site insiel.com.br gera: pedidos de orçamento, chamados técnicos e contatos.
Página única (`index.html`) publicada no GitHub Pages (repo `mesa-insiel`), banco no Supabase (projeto ABRAPE SOLAR `puepmnyffpfjttxmwfjt`, schema `insiel`).

- Login: Supabase Auth (e-mail + senha). Só e-mails na tabela `insiel.usuarios` (ativos) veem dados (RLS via `insiel.autorizado()`; admin via `insiel.admin()`).
- Tabelas: `leads` (site grava via RPC pública), `lead_eventos` (histórico/observações; trigger registra mudanças de status, responsável e visita), `usuarios`, view `v_resumo`.
- Requisito de configuração no painel Supabase: **Project Settings → Data API → Exposed schemas → adicionar `insiel`** e **Authentication → URL Configuration → Site URL / Redirect URLs** com o endereço da Mesa.
- Preview local: launch.json `mesa-insiel` (porta 8324). Deploy: `git push`.

## v2 (30/09/2026): chamados pela Mesa + Financeiro
- **Novo chamado** direto na Mesa (botão em Chamados). Status de chamado: Aberto (novo), Em atendimento (em_contato), Concluído · em cobrança (concluido), Faturado, Cancelado (descartado).
- **Compras** (`insiel.compras`): sempre vinculadas a um chamado aberto; solicitante cadastra finalidade/fornecedor/valor + cotação (Storage bucket privado `insiel`, pasta `compra/<id>/`). Admin aprova/recusa (trigger `compra_mudanca` registra quem/quando). Aprovada = despesa do chamado.
- **Contas a pagar** (`insiel.contas_pagar`): parcelas lançadas na compra aprovada (mensais a partir do 1º vencimento). Baixa de pagamento só papel financeiro/admin (policy `cp_upd`).
- **Contas a receber** (`insiel.contas_receber`): criada automaticamente (status a_definir) quando o chamado vai para Concluído (trigger `chamado_concluido`). Financeiro define valor/vencimento, pode dividir em parcelas, marca recebido; com todas pagas o chamado vira Faturado (trigger `cr_mudanca`).
- **Documentos** (`insiel.documentos`): cotação, NF, boleto, comprovante, ligados a compra ou conta; abertos por URL assinada (10 min).
- Papéis: admin (tudo + aprovar), financeiro (cobrança e baixas), atendimento (chamados, compras, documentos).
- Views: `v_compras`, `v_chamado_custos`, `v_contas_pagar`, `v_contas_receber`.
