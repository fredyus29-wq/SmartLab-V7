# Training Hub — Requisitos e Definições

## Visão
Centro de formação integrado que combina funcionalidades de LMS, geração de conteúdo assistida por IA e integração direta com QMS para evidências e bloqueios operacionais.

## Funcionalidades principais
- Course Authoring (slides, texto, multimédia, versionamento).
- Geração automática de slides e quizzes a partir de SOPs (IA).
- Avaliação automática e scoring; tracking de tentativas.
- Emissão de certificados (PDF assinados) e evento para QMS.
- Integração de evidências: logs e provas armazenadas no audit trail.

## Requisitos técnicos
- Serviço de IA desacoplado (workers que processam documentos e geram outputs).
- Filas (BullMQ) para orquestrar tarefas longas (geração de conteúdos, processamento de áudio/video).
- Persistência de conteúdos e metadados (DB + object storage).

## Fluxos críticos
1. Autoria → Publicação
2. Utilizador completa → Score calculado
3. Certificado gerado → Evento enviado ao QMS

## Critérios de maturidade para extração
- Alto consumo de CPU/GPU (IA) ou requisito de deploy separado.
- Necessidade de escalar independentemente do core.

