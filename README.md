# Portal de Orçamento — JR Soluções Elétricas

> Desenvolvido por **VF. Serviços e Soluções**

Sistema web de geração de orçamentos e ordens de serviço para a JR Soluções Elétricas. Roda como um arquivo HTML único, sem necessidade de instalação, acessível em computador e celular com dados sincronizados em tempo real via Firebase.

---

## Visão Geral

- **Arquivo principal:** `Portal_de_Orcamento_JR_V2.html`
- **Versão atual:** 2.0.0
- **Tecnologias:** HTML · CSS · JavaScript · Firebase Firestore · jsPDF · html2canvas
- **Modelo de entrega:** arquivo `.html` único, sem servidor próprio

---

## Funcionalidades

### Orçamento
- Preenchimento de dados do cliente com máscara de telefone e e-mail em minúsculas
- Planilha de serviços com cálculo automático de total
- Planilha opcional de materiais (Mão de Obra + Materiais)
- Numeração automática de documentos por ano (`JR-AAAA-NNNNN`)
- Textos de apresentação e escopo dos serviços
- Campo de observações
- Geração de PDF do orçamento com layout profissional

### Ordem de Serviço
- Vinculada ao mesmo número do orçamento
- Dados de execução: data, técnico responsável, cliente, endereço
- Registro fotográfico com legenda por foto (compressão automática)
- Grade de 6 fotos por página no PDF
- Geração de PDF da O.S. independente do orçamento

### Histórico
- Banco de dados em nuvem (Firebase Firestore) — mesmo dado acessível em qualquer dispositivo
- Busca por número, cliente ou endereço
- Filtro por status (Orçamento / Executado)
- Ações: Abrir PDF, Editar, O.S., Excluir

### Sistema de Licença
- Chave no formato `VF-XXXX-XXXX-XXXX`
- Validação local via HMAC-SHA256 com segredo embutido
- Trava por dispositivo: cada chave só funciona no aparelho em que foi ativada
- Expiração configurável por data
- Tela de ativação com exibição do Código do Dispositivo para o cliente enviar ao licenciador

### Atualizações Automáticas
- App verifica via API do GitHub se há versão nova ao ser aberto
- Exibe banner com novidades e link de download quando há atualização disponível

---

## Estrutura do Repositório

```
portal-jr-solucoes/
├── Portal_de_Orcamento_JR_V2.html   # App principal (entregue ao cliente)
├── version.json                      # Controle de versões para atualização automática
├── README.md                         # Este arquivo
└── CHANGELOG.md                      # Histórico de versões
```

> ⚠️ O arquivo `Gerador_de_Licencas_VF.html` **nunca deve ser adicionado a este repositório.** Ele é de uso interno exclusivo da VF. Serviços e Soluções.

---

## Como Atualizar o App

### 1. Incrementar a versão no código
No arquivo `Portal_de_Orcamento_JR_V2.html`, localize e atualize:
```js
const VERSAO_ATUAL = '2.0.0'; // altere aqui
```
E no sidebar do HTML:
```html
<span>V2.0</span> <!-- altere aqui -->
```

### 2. Atualizar o version.json
```json
{
  "versao": "2.1.0",
  "data": "DD/MM/AAAA",
  "novidades": "- Descrição da novidade 1\n- Descrição da novidade 2",
  "downloadUrl": "https://raw.githubusercontent.com/vgferraz/portal-jr-solucoes/main/Portal_de_Orcamento_JR_V2.html"
}
```

### 3. Subir os arquivos no GitHub
Faça upload (ou commit) dos arquivos atualizados:
- `Portal_de_Orcamento_JR_V2.html`
- `version.json`
- `CHANGELOG.md` (adicionar a nova versão)

O banner de atualização aparecerá automaticamente para o cliente na próxima vez que abrir o app.

---

## Firebase — Configuração

**Projeto:** `vf-jrsolucoeseletricas`
**Serviços utilizados:** Firestore Database · Authentication (Anônima)

### Estrutura do Firestore
```
orcamentos/
  {documentoId}/          # Ex: JR-2026-00001
    id, salvoEm, status, orcamento, os
    fotos/
      {0}, {1}, {2}...    # Cada foto da O.S. em documento separado
```

### Regras de Segurança (Firestore)
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /orcamentos/{orcamentoId} {
      allow read, write: if request.auth != null;
      match /fotos/{fotoId} {
        allow read, write: if request.auth != null;
      }
    }
  }
}
```

---

## Sistema de Licença — Fluxo de Venda

1. Cliente abre o app → tela de ativação exibe o **Código do Dispositivo** (8 caracteres)
2. Cliente envia o código + comprovante de pagamento para a VF.
3. VF. abre o `Gerador_de_Licencas_VF.html` (uso interno), informa o código do dispositivo, identificador do cliente e prazo
4. Gerador produz a chave `VF-XXXX-XXXX-XXXX`
5. VF. envia a chave para o cliente
6. Cliente digita a chave na tela de ativação → app liberado

> A chave só funciona no dispositivo cujo código foi usado na geração. Se o cliente trocar de aparelho, é necessário gerar uma nova chave com o código do novo dispositivo.

---

## Contato

**VF. Serviços e Soluções**
*(e-mail e WhatsApp a definir)*
