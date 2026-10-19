# MCI — instruções para agentes

## Escopo

Este arquivo se aplica a toda a árvore `MCI/`.

Leia este arquivo antes de alterar códigos, dados, notebooks ou o relatório. Preserve as alterações do usuário e não sobrescreva resultados existentes sem pedir.

**Regra permanente de memória:** este arquivo é a memória de longo prazo do projeto. Sempre que houver avanço (novo enunciado, decisão do usuário, script ou notebook novo, resultado obtido, mudança de estado de uma etapa), atualize este arquivo **na mesma sessão**, antes de encerrar o trabalho. O usuário usa este arquivo para recuperar o contexto quando a conversa é resetada, então ele deve refletir o estado real do projeto: o que já foi feito, os resultados atuais, as decisões tomadas e o que falta.

## Contexto

- Disciplina: **Métodos Computacionalmente Intensivos em Estatística (MCI)**, curso de Estatística, DEs – UFSCar, 2026.
- Professora: Daiane A. Zuanetti.
- Aluno: Giovani Verotti.
- Repositório: https://github.com/gverotti/MCI (branch `main`).
- Idioma do projeto: português (código pode ter nomes em português).

## Trabalho 1 (atual): `Material/Projeto Trabalhos/Trabalho 1/Trabalho_1_2026.pdf`

**Formato:** grupos de 2 pessoas (excepcionalmente 3). **A entrega é só de slides, sem relatório escrito.** A professora corrige pelos slides, então eles precisam ser autoexplicativos. Alguns grupos são sorteados para apresentar no horário da aula.

**Dados:** cada grupo coleta um conjunto de dados que permita aplicar reamostragem e permutação em pelo menos um destes cenários:
- 2 populações independentes;
- 2 populações pareadas;
- K > 2 populações independentes;
- regressão linear ou generalizada.

Pode ser uma base usada em outra disciplina. Sites sugeridos: r-bloggers (datasets), kdd.ics.uci.edu, csie.ntu.edu.tw/~cjlin/libsvmtools/datasets, kaggle.com/datasets, statsci.org/data. **Dois grupos não podem usar a mesma base**, por isso o grupo precisa avisar a turma qual vai usar.

**Conteúdo obrigatório dos slides** (checklist da professora):
1. Descrição de cada variável aleatória: local de coleta, forma de coleta, unidade de medida e o link, se vier de um site.
2. Uma tabela com os dados, ou com parte deles.
3. Objetivo(s) da análise descritos com clareza.
4. Análise descritiva **antes** dos testes (gráficos e medidas resumo), mostrando pontos atípicos, indicando as conclusões prováveis e **justificando o uso de métodos não paramétricos** em vez de paramétricos.
5. Hipóteses em termos matemáticos, em palavras e em termos do problema.
6. Suposições de cada metodologia, com justificativa **estatística** (não vale "porque achei melhor").
7. Verificação das suposições, quando possível, com gráficos ou medidas resumo.
8. Interpretação e conclusão.

**Análise exigida:** fazer as três versões, **paramétrica, permutação e bootstrap** (testes e/ou ICs). Depois comparar os resultados, discutir diferenças, vantagens e desvantagens, e **escolher o método mais adequado com justificativa**. A escolha de **B** deve ser justificada rodando o algoritmo para vários valores de B e mostrando a estabilização.

**Prazo no PDF:** slides até **16/outubro, 8:00h**. ATENÇÃO: o PDF foi lido em 19/10/2026, depois dessa data. É preciso confirmar com o usuário o prazo real (adiamento? data de outro ano?).

**Regras de nota zero:** trabalho muito parecido com estudos da literatura, livros, internet **ou plataformas de IA**. Portanto a base, as perguntas, as interpretações e o texto dos slides precisam ser do grupo. O agente apoia com código, conferência dos métodos e revisão, mas não deve entregar texto pronto e genérico para os slides.

**Decisões do usuário:**
- **Linguagem: Python, obrigatoriamente** (decidido em 19/10/2026). Não usar R.
- **Base: Breast Cancer Wisconsin (Original)**, UCI: https://archive.ics.uci.edu/dataset/15/breast+cancer+wisconsin+original. Detalhes na seção "Dados".

**Pendências de definição com o usuário:** prazo real, dupla, se a base já foi avisada à turma, qual dos 3 arquivos usar (original × WDBC), cenário/pergunta, tratamento dos IDs repetidos e ferramenta de slides.

## Dados (`dados/breast+cancer+wisconsin+original/`)

Pasta baixada inteira do UCI (validada em 19/10/2026). Ela contém **3 bases diferentes**:

| Arquivo | Base | n | Variáveis | Faltantes |
|---|---|---|---|---|
| `breast-cancer-wisconsin.data` | **Original** (o link escolhido) | 699 | id + 9 escores **ordinais inteiros 1–10** + classe (2 = benigno, 4 = maligno) | 16 (`?` em `bare_nuclei`; 14 benignos, 2 malignos) |
| `wdbc.data` | WDBC, diagnóstico (Street, 1995) | 569 | id + diagnóstico (B/M) + 30 medidas **contínuas** de núcleos celulares (média, EP e "pior" de raio, textura, perímetro, área, etc.) | 0 |
| `wpbc.data` | WPBC, prognóstico (N/R = não recorrente / recorrente) | 198 | id + desfecho + tempo + 32 atributos | 4 |

`unformatted-data` = a base original separada nos **8 grupos cronológicos de coleta** (jan/1989 a nov/1991), com a documentação de cada grupo.

**Resultado da validação da base original:**
- Bate com o `.names`: 699 linhas, 458 benignos (65,5%) e 241 malignos (34,5%), 16 faltantes, todos os atributos entre 1 e 10 (`mitoses` tem só 9 valores distintos).
- **O `id` não é único:** são 645 ids distintos e 54 linhas com id repetido. Desses ids, 8 aparecem em pares de linhas idênticas, 39 têm versões diferentes dos atributos e **4 aparecem com classes diferentes**. Isso ameaça a suposição de independência e intercambiabilidade, então é preciso decidir o tratamento (manter, deduplicar ou ficar com 1 por id) e justificar nos slides.
- As variáveis são **ordinais**, com muitos empates e concentração em 1 nos benignos. Medianas benigno × maligno: clump 3×8, unif_cell_size 1×6, unif_cell_shape 1×6, marginal_adhesion 1×5, single_epith 2×5, bare_nuclei 1×10, bland_chromatin 2×7, normal_nucleoli 1×6, mitoses 1×1.
- Implicações para o trabalho:
  - (a) Benigno × maligno é uma diferença enorme, então qualquer teste dá valor-p ≈ 0 e a comparação entre métodos fica pouco informativa.
  - (b) Com n grande, o TCL enfraquece o argumento contra o teste paramétrico (item 4 do checklist).
  - (c) A escala ordinal 1–10 torna a "média" discutível.
  - (d) É uma das bases mais conhecidas da internet, o que aumenta o risco das regras "mesma base de outro grupo" e "parecido com estudos disponíveis".
- Origem, para o item 1 dos slides: University of Wisconsin Hospitals, Madison, casos clínicos do Dr. William H. Wolberg, coletados de 1989 a 1991. Os escores vêm de citologia por aspiração com agulha fina (FNA), atribuídos pelo médico.

**WDBC (verificação rápida):** 569 ids únicos, 357 B e 212 M, sem faltantes. As medidas são contínuas e várias são assimétricas e não normais por classe (Shapiro: área com p < 1e-7 nos malignos, assimetria 1,11).

## Listas de exercícios (`Material/Listas de Exercicios/`)

Não fazem parte do Trabalho 1, mas mostram o padrão de resposta que a professora espera: formular as hipóteses (no problema e em termos estatísticos), definir a variável aleatória, construir a RC, calcular o valor-p, concluir e checar as suposições.

- **Lista 1** (Unidades 02–03):
  - Ex1: outras estatísticas nos exemplos de permutação.
  - Ex2: teste de permutação para igualdade de variâncias.
  - Ex3: pressão arterial pareada (n = 11): permutação, bootstrap e teste paramétrico.
  - Ex4: droga × placebo, 5 e 5, independentes: permutação, bootstrap e paramétrico.
  - Ex5: IC bootstrap para a correlação de Pearson com B = 100, 500, 1000 e 5000, numa relação não linear.
  - Ex6: jackknife × bootstrap na base law.
  - Ex7: vício de S² com n e com n−1 e correções bootstrap e jackknife, N(0, 10).
  - Ex8: 100 amostras N(1, 1) com n = 20; cobertura dos ICs paramétrico, bootstrap NP, bootstrap paramétrico e jackknife.
  - Ex9: dose-resposta em porcos; MQ, bootstrap dos resíduos × bootstrap dos pares, testando H0: β = 20.
- **Lista 2** (Unidade 04):
  - Ex1: método delta para τ = p2 − p1 (30/50 × 40/50), IC 90%.
  - Ex2: IC para μ2/μ1 pelo método delta.
  - Ex3: delta para n/(Y+1), Poisson.
  - Ex4: Gama(α, α/μ), EMV e escore de Fisher para μ, para α e para os dois, com simulação.
  - Ex5: EM para o modelo de intercepto aleatório Yij = αi + Xijβ + εij.

## Material da professora (`Material/`)

Lido integralmente em 19/10/2026. Slides em PDF + códigos em R (salvos como `.txt` em `Material/Codigos R/`). A Unidade 01 não está na pasta.

### Unidade 02: testes de permutação (`02_Met_permut.pdf`, `teste_permutacao.txt`)
- Ideia: a distribuição da estatística sob H0 é obtida recalculando-a em todos os rearranjos dos dados. Suposição: as observações são intercambiáveis sob H0.
- **Pareado:** Z = Y − X, estatística T1 = Z̄. Sob H0 os sinais das diferenças são trocáveis, então há 2^n rearranjos. Exemplo do tranquilizante (escore de Hamilton, n = 9): Z̄obs = −0,43, RC = {Z̄ ≤ −0,33}, valor-p = 0,014 (unilateral, H1: M < 0). O código faz as 512 combinações com 9 `for` aninhados.
- **Independentes:** T1 = ΣXi, com C(n+m, n) partições. Exemplo x = (0,1,1,0,−2), y = (6,7,7,4,−3,9,14): 792 partições, T1obs = 0, RC = {T1 ≤ 2 ou T1 ≥ 35}, valor-p bilateral = 0,028 (= 2·P(T1 ≤ T1obs)). O código usa 5 `for` aninhados com o limite `12` fixo.
- Outras estatísticas: X̄ − Ȳ, estatística t, diferença de medianas, razão de variâncias. Quando a enumeração é inviável, usar permutações aleatórias.
- **K > 2 grupos:** permutar a amostra combinada entre os grupos e usar a estatística F da ANOVA. Valor-p = proporção de F* ≥ Fobs. (Só nos slides, sem código.)

### Unidade 03: reamostragem (`03_Met_reamost.pdf`, `metodos_reamostragem.txt`)
- Princípio plug-in (F̂ empírica no lugar de F); desigualdade DKW.
- **Bootstrap** (Efron, 1979): erro padrão bootstrap EPB, vício bootstrap θ̂*(·) − θ̂, estimador corrigido 2θ̂ − θ̂*(·). Escolha de B: cerca de 50–200 para o EP, muito mais para IC. Versões não paramétrica, paramétrica e suavizada.
- **Exemplos no código (R, `set.seed(100)`):**
  - correlação LSAT × GPA (`library(bootstrap)`, dados `law` com n = 15 e `law82` com a população N = 82; B = 3200). Compara a distribuição bootstrap com a distribuição de amostras iid da população; ρ populacional ≈ 0,76;
  - correção do vício de max(X) numa Uniforme(0, θ) (n = 15, depois 100; B = 5000);
  - IC normal (θ̂ ± 1,96·EPB) × IC percentil para a correlação;
  - IC percentil para a diferença de médias de duas Gamas (n = 20, depois 100);
  - teste bootstrap com 1 amostra: tempos de vida de 7 ratos, H0: μ = 129 × H1: μ < 129, dados transladados ỹ = y − ȳ + 129, T(y) = −1,67;
  - regressão (octanagem × % de aditivo, x = 1..6): bootstrap dos **resíduos** × bootstrap dos **pares**, B = 3000, IC percentil para α e β.
- Nos slides, mas sem código: teste bootstrap de 2 médias (translada as duas amostras para a média combinada z̄, estatística de Welch, valor-p bilateral com |T|).
- Comparação permutação × bootstrap: a permutação dá valor-p exato e costuma ser mais poderosa; o bootstrap dá valor-p aproximado.
- **Jackknife:** vício_jack = (n−1)(θ̂(·) − θ̂), EP_jack = √[(n−1)/n · Σ(θ̂(i) − θ̂(·))²]. Validação cruzada (holdout, k-fold, LOO) só é citada. (Sem código.)

### Unidade 04: otimização estatística (`04_Met_otim.pdf`, `metodos_otimizacao.txt`)
- **Método delta**, uni e multivariado. Exemplos: exp(X̄); p̂/(1−p̂), com Var ≈ p / [n(1−p)³]; X̄1·X̄2, com variância μ2²σ11 + 2μ1μ2σ12 + μ1²σ22. (Sem código.)
- **Newton-Raphson / escore de Fisher:** θ_{m+1} = θ_m + I⁻¹(θ_m)V(θ_m), parando quando ||θ_{m+1} − θ_m|| < ε.
  - Código: Gama(α, β) com β conhecido (taxa β = 2, α0 = 0,1, ε = 1e-6; "depois trocar para α = 10, β = 1, chute inicial 10").
  - Código: α e β desconhecidos (vetor escore com nψ'(α), I12 = −n/β, I22 = nα/β², chute inicial (1, 1), ε = 1e-5; "depois α = 5, β = 5").
- **Algoritmo EM** (Dempster, Laird e Rubin, 1977): passo E (Q = E[l completa | y]) e passo M.
  - Mistura de K normais: p_ik = w_k f_k(y_i) / Σ_j w_j f_j(y_i); μ_k e σ²_k ponderados por p_ik; w_k = Σ_i p_ik / n (via Lagrange). Critério de parada sugerido: aumento da log-verossimilhança < ε.
  - Código: simula n = 1000 com w = (0,2; 0,3; 0,5), μ = (1, 2, 3), σ = (0,1; 0,2; 0,3), chute w = 1/3, μ = (−1, 0, 1), σ² = 1, e roda **200 iterações fixas** (sem critério de parada). Mostra as trajetórias, a densidade ajustada e a tabela de grupos preditos × verdadeiros.
  - Exercício deixado no final do código: trocar os parâmetros para o **exemplo das alturas** dos slides (40% homens N(1,80; 0,1²), 40% mulheres N(1,60; 0,05²), 20% crianças N(1,20; 0,13²)) e avaliar o desempenho.

### Observações sobre o material
- No slide 15 da Unidade 02, o valor-p bilateral aparece como "2 × #(T1 ≥ T1obs)". O código usa 2 × #(T1 ≤ T1obs), que é o correto nesse exemplo, porque T1obs está na cauda inferior. O certo, em geral, é 2 × min das duas caudas.
- Nos `metodos_reamostragem` em R, os dados `law` e `law82` vêm do pacote `bootstrap`. Em Python será preciso trazer esses dados manualmente.
- No EM, a tabela `Grupo × S_verd` pode sofrer troca de rótulos (*label switching*), conforme o chute inicial.

## Estrutura

```
MCI/
├── Material/    # slides e códigos R da professora
├── dados/       # bases de dados (brutas e tratadas)
├── scripts/     # códigos em R / Python
├── notebooks/   # Jupyter / R Markdown / Quarto
├── figuras/     # gráficos gerados
├── relatorio/   # relatório final
├── AGENTS.md    # este arquivo
├── README.md
├── requirements.txt
└── .gitignore
```

## Ambiente Python

- `.venv/` local (fora do git), Python 3.12.10 (`C:\Users\Giovani Verotti\AppData\Local\Programs\Python\Python312`).
- `requirements.txt` é uma **cópia exata** (`pip freeze`, 138 pacotes) do `.venv` do `../Modelagem_Logistico`, feita a pedido do usuário. Principais: numpy 1.26.4, scipy 1.17.1, pandas 3.0.5, polars 1.43.2, scikit-learn 1.9.0, statsmodels 0.14.6, matplotlib 3.11.1, numba 0.68.0, networkx, igraph, cvxpy, optbinning, skglm, jupyter.
- Kernel Jupyter registrado como **"Python (MCI)"** (nome `mci`).
- Recriar: `python -m venv .venv` → `.venv\Scripts\python -m pip install -r requirements.txt`.
- Ao instalar uma lib nova, atualizar o `requirements.txt` (`pip freeze > requirements.txt`, mantendo o cabeçalho) e anotar aqui.

## Convenções

- Simulações devem fixar semente (`set.seed` / `numpy.random.default_rng(seed)`) e registrá-la, para que os resultados sejam reproduzíveis.
- Figuras geradas vão para `figuras/`, com nomes descritivos.
- Commits com mensagens em português, descrevendo o que mudou.
- Fluxo git: `git add .` → `git commit -m "..."` → `git push`.

## Estado atual (19/10/2026)

- Repositório criado e publicado no GitHub com a estrutura de pastas, o README e o `.gitignore` (R, Python, Jupyter, Quarto, LaTeX).
- O README criado pelo GitHub foi mesclado (`--allow-unrelated-histories`), mantendo a versão local.
- Ambiente virtual `.venv` criado e verificado (mesmas versões do Modelagem_Logistico; imports testados), kernel "Python (MCI)" registrado.
- Material da professora (Unidades 02, 03 e 04) adicionado pelo usuário em `Material/` e lido; o resumo está acima.
- Listas 1 e 2 e o enunciado do Trabalho 1 adicionados e lidos (resumo acima). Não havia códigos novos além dos 3 de `Codigos R/`.
- Nenhum código nosso ou dado ainda.

## Próximos passos

1. Confirmar o prazo real do Trabalho 1 (o PDF diz 16/out, 8:00h).
2. Definir: arquivo (original × WDBC), cenário e pergunta, tratamento dos ids repetidos; avisar a turma da base.
3. Definir a ferramenta dos slides.
4. Montar o roteiro de análise seguindo o checklist de 8 itens + comparação paramétrico × permutação × bootstrap + escolha de B.

## Histórico

- **19/10/2026:** criação da pasta `MCI/`, `git init`, primeiro commit, ligação ao remote `gverotti/MCI` e push. Criação deste `AGENTS.md`. Criação do `.venv` + `requirements.txt` copiando o ambiente do Modelagem_Logistico. Leitura completa do `Material/` (Unidades 02–04 + códigos R). Leitura das Listas 1 e 2 e do enunciado do Trabalho 1. Usuário escolheu Python e a base Breast Cancer Wisconsin (Original); validação feita (ids repetidos, 16 faltantes, pasta contém também WDBC e WPBC).
