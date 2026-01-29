# UX e Requisitos de Produto — SmartLab

## Princípios UX
- Experiência unificada: para o cliente que comprou tudo, o SmartLab parece um único sistema.
- Login único, Shell App com menu dinâmico por módulos/roles.
- Interface SAP‑like com densidade de informação e foco em produtividade.

## Estrutura do Frontend
- `shell` central que fornece autenticação, contexto do tenant, tema e menu.
- Módulos independentes carregados por feature flags/licença.
- Monorepo recomendado inicialmente, com possibilidade de split futuro.

## Fluxos UX importantes
- Menu dinâmico com composição automática a partir do payload da API.
- Lazy loading dos módulos e rotas protegidas por guards e roles.
- Workflows visuais para auditoria, CAPA e aprovação de documentos.

## Acessibilidade e internacionalização
- Design tokens para cores, espaçamento e densidade.
- i18n desde o início (PT/EN obrigatórios para mercados alvo).

## Benefícios de Produto
- Venda modular transparente ao cliente.
- Upsell fácil via ativação de módulos por licença.
- Onboarding guiado e templates para SOPs e checklists.
