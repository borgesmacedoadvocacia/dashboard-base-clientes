# Dashboard — Base de Clientes

Painel da carteira de clientes do escritório Borges Macedo Advocacia, alimentado em tempo
real pela planilha **"Base de Clientes PF e PJ"** do Google Sheets.

**Acesso exclusivo das lideranças.** Só os perfis **Administração** e **Lideranças** têm
cofre neste painel.

## O que é exibido

| Seção | Conteúdo |
|---|---|
| Resumo Executivo | Total de clientes, integridade do cadastro, documentos inválidos, duplicidades, e-mails e telefones suspeitos, estados alcançados e leads sem identificação |
| Composição e Alcance | PF × PJ, clientes por estado (DDD como proxy da praça) e tabela de concentração geográfica com participação acumulada |
| Qualidade do Cadastro | Cartões de integridade e tabela filtrável por tipo de ocorrência (documento inválido, duplicidade de CPF/CNPJ, telefone ou e-mail, contato suspeito) |
| Carteira PJ | Empresas, representantes e destaque para representantes que **também** são clientes pessoa física |
| Contatos Não Identificados | Leads do Kommo cruzados por telefone com a base — separa quem já é cliente de quem precisa ser enriquecido |
| Pontos de Atenção | Leitura executiva automática dos números |

## Validações aplicadas

- **CPF e CNPJ**: conferidos pelos dígitos verificadores (não é só contagem de dígitos).
- **Telefone**: DDD tabelado por estado; aceita 8 ou 9 dígitos e prefixo `55`.
- **E-mail**: formato mais erros de digitação frequentes em domínio (`.con`, `.copm`,
  `.cim`, `@gmail.co`).
- **Duplicidade**: mesmo CPF/CNPJ, mesmo telefone ou mesmo e-mail em mais de uma linha.
- **Cruzamento PF × PJ**: representante de empresa que também consta como cliente pessoa
  física, por documento ou por nome normalizado (sem acento e sem variação de caixa).
- **Cruzamento Kommo × base**: telefone normalizado, para separar o lead que já é cliente
  daquele que realmente está fora da base.

## Como funciona

- Lê as abas **Clientes PF**, **Clientes PJ** e **Kommo (Sem identificação)** pela
  **Sheets API v4** (`batchGet`), sempre por nome de cabeçalho com fallback por coluna.
- Atualização automática a cada 5 minutos e ao voltar para a aba do navegador.
- Arquivo único `index.html`, sem build e sem servidor.

> O DDD é um **proxy** da praça: indica de onde o cliente fala, não onde ele mora. Serve
> para planejar capilaridade e rede de correspondentes, não para endereço processual.
