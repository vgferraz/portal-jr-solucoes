# Changelog — Portal de Orçamento JR Soluções Elétricas

Todas as mudanças relevantes do projeto são documentadas aqui.
Formato baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/).

---

## [2.0.0] — 2026-06-18

### Adicionado
- **Ordem de Serviço (O.S.)** vinculada ao mesmo número do orçamento
- **Registro fotográfico** na O.S. com compressão automática de imagens e legenda por foto
- **PDF independente da O.S.** com grade de 6 fotos por página, dados de execução e espaço para assinaturas
- **Histórico de orçamentos** com busca, filtro por status e ações (Abrir, Editar, O.S., Excluir)
- **Banco de dados em nuvem** via Firebase Firestore — dados sincronizados entre celular e computador
- **Autenticação anônima** para acesso seguro ao Firestore sem login do usuário
- **Regras de segurança** no Firestore (acesso bloqueado para requisições não autenticadas)
- **Sistema de licença** com chave `VF-XXXX-XXXX-XXXX` validada via HMAC-SHA256
- **Trava por dispositivo** — cada chave só funciona no aparelho em que foi ativada
- **Gerador de Licenças** (`Gerador_de_Licencas_VF.html`) para uso interno da VF. Serviços e Soluções
- **Atualizações automáticas** via API do GitHub com banner de notificação e link de download
- Botão **"🏠 Início"** fixo na topbar e barra mobile para voltar à tela inicial de qualquer tela
- Botão **"📂 Histórico"** na topbar e barra mobile
- Botão **"✏️ Editar"** no Histórico para recarregar orçamento salvo no formulário
- Botão **"💾 Salvar"** salva apenas os dados sem gerar PDF
- **"Emitir Orçamento"** gera PDF, salva no Histórico e reinicia o formulário com novo número
- Fotos da O.S. salvas em subcoleção separada no Firestore (`orcamentos/{id}/fotos`) para respeitar o limite de 1MB por documento
- Copiar código do dispositivo e chave de licença com fallback para contexto `file://` (sem HTTPS)

### Alterado
- Prefixo das chaves de licença alterado de `JR-` para `VF-` (marca do licenciador)
- Banco de dados migrado de `localStorage` para Firebase Firestore
- Botões **"PDF Orçamento"** e **"PDF Ordem de Serviço"** renomeados para **"Emitir Orçamento"** e **"Emitir O.S."**
- Botão de Ordem de Serviço removido da topbar — acesso agora pelo Histórico ou pela própria tela
- Tela de licença exibe "Licenciado por VF. Serviços e Soluções"

### Removido
- Indicador de armazenamento local (obsoleto após migração para nuvem)
- Embutimento de PDF no registro do banco (resolvia estouro de armazenamento)

---

## [1.0.0] — 2025 (versão original)

### Adicionado
- Formulário de orçamento com dados do cliente (nome, telefone, e-mail, endereço)
- Planilha de serviços com cálculo automático de total
- Planilha opcional de materiais (Mão de Obra + Materiais)
- Numeração automática de documentos no formato `JR-AAAA-NNNNN`
- Textos de apresentação e escopo dos serviços
- Campo de observações
- Biblioteca de serviços pré-cadastrados
- Geração de PDF do orçamento em layout profissional (múltiplas páginas)
- Envio por WhatsApp
- Sidebar com informações da empresa e navegação
- Layout responsivo para desktop e mobile
- Barra de ações mobile fixa na parte inferior
