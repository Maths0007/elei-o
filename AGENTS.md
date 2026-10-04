# AGENTS.md — Apuração Eleitoral 2026

Instruções permanentes para qualquer agente que trabalhe neste repositório.

## Objetivo do projeto

Site (frontend + backend) que acompanha a apuração da eleição presidencial brasileira de 2026 **usando somente dados oficiais do TSE**, permitindo navegar do resultado geral até zona e seção eleitoral, incluindo a votação no exterior. Interface 100% em português do Brasil.

## Regras de dados (inegociáveis)

1. **Nunca invente** endpoints, URLs, códigos de eleição, campos ou valores. Descubra tudo a partir da documentação e das configurações oficiais do TSE.
2. Se não conseguir acessar a documentação ou um endpoint oficial, **pare e informe** o que faltou. Não preencha lacunas com suposições.
3. **Nunca simule votos** quando o TSE não fornecer um dado. Dado ausente é exibido como indisponível.
4. **Nenhum dado fictício em produção.** Mocks só em desenvolvimento, totalmente separados do código de produção e sempre com o aviso visual: `MODO DE DESENVOLVIMENTO — DADOS SIMULADOS`.
5. **Nunca altere dados recebidos** para fazer somas baterem. Inconsistências são registradas em log e marcadas como `Aguardando validação da fonte`.
6. Tipos TypeScript são criados **depois** de analisar respostas reais do TSE, nunca antes. Valide toda resposta externa com schema (ex.: Zod).
7. Candidatos, partidos e localidades são **carregados dinamicamente**; nenhum nome fixo no código.
8. Não faça scraping de portais jornalísticos. Fontes permitidas: domínios oficiais do TSE (allowlist).

## Neutralidade

- Mostre apenas números efetivamente publicados pelo TSE.
- Sem previsões, projeções, análises políticas, adjetivos ou avaliações sobre candidatos.
- Ordem alfabética por padrão; ordenações alternativas são apenas informativas.
- Paleta de cores neutra: não associar cores a candidatos ou partidos.
- Fotos de candidatos só com fonte oficial e licença verificável; senão, avatar neutro.

## Arquitetura

- Next.js + TypeScript, React, Tailwind CSS, rotas de API / backend Node.js.
- Fluxo obrigatório: `TSE → backend do site → cache/banco → frontend`. O navegador **nunca** consulta o TSE diretamente.
- Camada de integração isolada em `services/tse.ts` (configuração, códigos da eleição, resultados, progresso da totalização, localidades, boletins de urna, normalização).
- Cache com TTL: resultados gerais 30 s; candidatos, localidades e zonas 30 min ou mais. Usar stale-while-revalidate.
- Se o TSE falhar, manter o último dado válido e exibir: `Dados temporariamente indisponíveis para atualização. Exibindo a última informação recebida do TSE.`
- Ingestão incremental e **idempotente**: chave composta (eleição + turno + UF/país + zona + seção, adaptada aos identificadores reais). Reprocessar um boletim nunca duplica votos.
- Atenção à Vercel: sem disco persistente e com tempo limite por função. Ingestão pesada roda em job/worker/cron, não em requisição de usuário.

## Validações

- `votos_candidatos + brancos + nulos <= comparecimento`, além de outras checagens possíveis com a estrutura oficial.
- Comparar totais calculados com totais agregados publicados pelo TSE e exibir divergências sem corrigi-las.
- Status técnico de seção: `aguardando resultado`, `resultado recebido`, `atualizado`, `erro de processamento`. Não usar para inferir resultado eleitoral.
- Só exibir "100% apurado" se o TSE indicar isso.

## Segurança

- Allowlist de domínios oficiais do TSE; parâmetros do usuário **nunca** definem URL consultada no servidor.
- Validar todas as entradas; proteger contra XSS, injection e parâmetros manipulados.
- Rate limiting por IP; cabeçalhos de segurança (CSP, HSTS, X-Content-Type-Options).
- Segredos só em variáveis de ambiente; manter `.env.example` atualizado e nunca commitar `.env`.
- Não coletar dados pessoais nem usar rastreamento sem aviso.

## Interface

- Desktop e mobile; identidade visual própria (não copiar o site do TSE).
- Acessibilidade (WCAG): contraste, teclado, leitor de tela, alternativa em tabela para gráficos.
- Estados de carregamento, vazio e erro em todos os componentes.
- URLs compartilháveis que refletem os filtros.
- Em toda tela com dados: "Fonte: Tribunal Superior Eleitoral — TSE" e o horário em que o dado foi obtido (horário de Brasília).
- Página "Sobre os dados" com o texto: "Este site não realiza apuração própria. Os números exibidos são obtidos de dados oficiais disponibilizados pelo Tribunal Superior Eleitoral (TSE)."

## Forma de trabalho

- Trabalhe **em etapas pequenas** e pare ao final de cada uma para revisão.
- Escreva testes: parsing/normalização, idempotência da ingestão, validações, e testes de contrato que falhem se o formato do TSE mudar.
- Rode lint, typecheck e testes antes de concluir qualquer tarefa.
- Registre decisões e descobertas em `docs/` (ex.: `docs/tse-formatos.md`, `ARCHITECTURE.md`).
- Código e comentários técnicos podem ser em inglês; textos da interface e documentação para o usuário em português do Brasil.
