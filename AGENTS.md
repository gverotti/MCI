# MCI — instruções para agentes

## Escopo

Este arquivo se aplica a toda a árvore `MCI/`.

Leia este arquivo antes de alterar códigos, dados, notebooks ou o relatório. Preserve as alterações do usuário e não sobrescreva resultados existentes sem pedir.

**Regra permanente de memória:** este arquivo é a memória de longo prazo do projeto. Sempre que houver avanço (novo enunciado, decisão do usuário, script ou notebook novo, resultado obtido, mudança de estado de uma etapa), atualize este arquivo **na mesma sessão**, antes de encerrar o trabalho. O usuário usa este arquivo para recuperar o contexto quando a conversa é resetada, então ele deve refletir o estado real do projeto: o que já foi feito, os resultados atuais, as decisões tomadas e o que falta.

## Contexto

- Disciplina: **Métodos Computacionais Intensivos (MCI)**, curso de Estatística.
- Aluno: Giovani Verotti.
- Repositório: https://github.com/gverotti/MCI (branch `main`).
- Idioma do projeto: português (código pode ter nomes em português).

## Objetivo do trabalho

*Ainda não definido.* O enunciado do trabalho ainda não foi passado. Quando chegar, registrar aqui: tema, entregáveis, prazo, linguagem exigida (R/Python) e critérios de avaliação.

## Estrutura

```
MCI/
├── dados/       # bases de dados (brutas e tratadas)
├── scripts/     # códigos em R / Python
├── notebooks/   # Jupyter / R Markdown / Quarto
├── figuras/     # gráficos gerados
├── relatorio/   # relatório final
├── AGENTS.md    # este arquivo
├── README.md
└── .gitignore
```

## Convenções

- Simulações devem fixar semente (`set.seed` / `numpy.random.default_rng(seed)`) e registrá-la, para que os resultados sejam reproduzíveis.
- Figuras geradas vão para `figuras/`, com nomes descritivos.
- Commits com mensagens em português, descrevendo o que mudou.
- Fluxo git: `git add .` → `git commit -m "..."` → `git push`.

## Estado atual (19/10/2026)

- Repositório criado e publicado no GitHub com a estrutura de pastas, o README e o `.gitignore` (R, Python, Jupyter, Quarto, LaTeX).
- O README criado pelo GitHub foi mesclado (`--allow-unrelated-histories`), mantendo a versão local.
- Nenhum código ou dado ainda.

## Próximos passos

1. Receber o enunciado do trabalho e registrar na seção "Objetivo do trabalho".
2. Definir a linguagem (R ou Python) e o formato do relatório.

## Histórico

- **19/10/2026:** criação da pasta `MCI/`, `git init`, primeiro commit, ligação ao remote `gverotti/MCI` e push. Criação deste `AGENTS.md`.
