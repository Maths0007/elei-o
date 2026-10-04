# Prompt — Etapa 1: Descoberta dos dados oficiais do TSE

Cole o texto abaixo no Codex (com o `AGENTS.md` já na raiz do repositório).

---

Leia o `AGENTS.md` e siga todas as regras dele.

**Esta etapa é somente de pesquisa e documentação. Não escreva código da aplicação, não instale dependências e não crie componentes.**

## Tarefa

Descubra e documente, a partir de fontes oficiais do TSE, como os dados da eleição presidencial brasileira de 2026 são divulgados.

1. Consulte a documentação técnica oficial atual do TSE ("Informações técnicas sobre a divulgação de resultados 2026" ou o nome equivalente atual) e o Portal de Dados Abertos do TSE.
2. Identifique e liste, com URL oficial completa:
   - arquivos/endpoints de **configuração** (como descobrir o código da eleição, turno, cargo Presidente, ciclo);
   - arquivos/endpoints de **acompanhamento da totalização** (progresso, seções totalizadas, total de seções);
   - arquivos/endpoints de **resultados** (candidatos, votos, brancos, nulos, válidos), nos níveis: Brasil, UF, município, exterior (país/localidade);
   - arquivos de **zonas e seções**, e de **Boletim de Urna** (formato, onde obter, como identificar UF/país, município/localidade, zona e seção);
   - conjuntos do **Portal de Dados Abertos** para votação por seção e boletins de urna.
3. Para cada item, registre: URL, método, formato, frequência de atualização informada, campos principais (nome e significado), padrão de nomenclatura de arquivos, e se há mecanismo para detectar atualização (hash, data, versão).
4. Baixe **uma amostra real** de cada tipo de arquivo que estiver disponível e guarde em `docs/amostras/` (se for muito grande, guarde só um trecho e registre o tamanho total). Se ainda não houver dados de 2026 publicados, use a amostra oficial mais recente equivalente (por exemplo, eleição de 2022) e **identifique claramente** o ano de cada amostra.
5. Explique como o **exterior** é representado nos dados reais (país, localidade, zona, seção), sem presumir estrutura.
6. Liste os domínios oficiais encontrados, que formarão a allowlist do backend.

## Entregáveis

- `docs/tse-formatos.md` — documento organizado com tudo acima, em português.
- `docs/amostras/` — amostras reais, com um `README.md` indicando origem (URL), data/hora do download e ano da eleição.
- `docs/lacunas.md` — tudo o que você **não conseguiu confirmar** (endpoints inacessíveis, campos ambíguos, dados ainda não publicados), com perguntas objetivas para mim.

## Regras desta etapa

- Não invente nada. Se não conseguir confirmar, escreva "não confirmado" e vá para `docs/lacunas.md`.
- Se não tiver acesso à internet ou aos domínios do TSE no ambiente, **pare e me diga exatamente o que está bloqueado**.
- Ao terminar, resuma em poucas linhas o que foi confirmado e o que falta, e **aguarde minha revisão** antes da próxima etapa.
