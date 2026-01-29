# Requisitos Funcionais — SmartLab

## Objetivo
Definir funcionalidades essenciais por domínio para suportar operações regulatórias e industriais.

## Requisitos por módulo

### 1. LIMS
- Registo e gestão de amostras (life cycle: recebimento → preparação → análise → retenção).
- Gestão de métodos de ensaio, validação de resultados e workflows de revisão/aprovação.
- Geração de COA (Certificate of Analysis) e exportação PDF/Excel.
- Integração com equipamentos (via ETL/ingest) e registo de metadados de análise.
- Detecção e sinalização de OOS/OOT (com suporte a IA para causa raiz).

### 2. QMS
- Gestão documental (versões, aprovações, distribuição).
- Registo de Não Conformidades (NC) e CAPA workflows.
- Matriz de competências e ligação ao TMS (formação exigida por função).
- Auditoria e evidências associadas a mudanças e aprovações.

### 3. FSMS
- Definição e manutenção de HACCP / PRPs / OPRPs.
- Monitorização contínua de CCPs e geração de alertas.
- Checklists e auditorias internas/externas.

### 4. TMS (Training Management)
- Authoring de cursos (slides, texto, multimédia), quizzes e avaliações.
- Emissão de certificados, tracking de progresso e integração com QMS.
- Integração com IA para geração automática de slides e quizzes a partir de SOPs.

### 5. Reports & Analytics
- Dashboards operacionais e relatórios regulatórios preparados para auditoria.
- Camada semântica de queries e views versionadas.
- Exportação para formatos padrão (PDF, Excel) e geração programada de reports.

## Requisitos transversais
- Multi‑tenant (opcional) com segregação clara de dados por tenant.
- RBAC e políticas de autorização por contexto.
- Workflows configuráveis por domínio.
- Evidência auditável para todas as ações críticas.

## Requisitos UX/Usabilidade
- Shell único com navegação consistente.
- Menus dinâmicos por licença/módulo e role.
- Experiência SAP‑like (densidade de informação, eficiência para power users).

## Integrações
- API‑first: endpoints REST/GraphQL bem documentados.
- Conectores para ERPs, equipamentos laboratoriais, sistemas de arquivos corporativos e serviços de identidade.

