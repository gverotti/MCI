# MCI — instruções para agentes

## Escopo

Este arquivo se aplica a toda a árvore `MCI/`.

Leia este arquivo antes de alterar códigos, dados, notebooks ou o relatório. Preserve as alterações do usuário e não sobrescreva resultados existentes sem pedir.

**Regra permanente de memória:** este arquivo é a memória de longo prazo do projeto. Sempre que houver avanço (novo enunciado, decisão do usuário, script ou notebook novo, resultado obtido, mudança de estado de uma etapa), atualize este arquivo **na mesma sessão**, antes de encerrar o trabalho. O usuário usa este arquivo para recuperar o contexto quando a conversa é resetada, então ele deve refletir o estado real do projeto: o que já foi feito, os resultados atuais, as decisões tomadas e o que falta.

**Sobre a memória do agente:** a memória automática do Claude Code é separada por pasta aberta no VS Code. O usuário às vezes abre `VSCODE/` (a pasta pai) e às vezes `MCI/` direto (Open Folder). Por isso **este arquivo é a única memória confiável**. O `CLAUDE.md` deste projeto importa este arquivo (`@AGENTS.md`) para que ele seja carregado nos dois casos. Registre aqui tudo o que for discutido e decidido; não confie na memória da conversa.

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

**Já resolvido pelo usuário (19/10/2026):** prazo, dupla, aviso da base à turma e ferramenta de slides. Os detalhes não foram passados ao agente. Se precisar deles, perguntar.

**Pendências de definição com o usuário:** confirmação da abordagem B e escolha da variável (depois da Etapa 1).

## Abordagens propostas para o Trabalho 1 (discutidas em 19/10/2026)

Contexto: na base original, benigno × maligno dá valor-p ≈ 0 em qualquer método, e a comparação entre métodos, que é o centro do trabalho, fica pouco informativa. Por isso foram propostos 3 caminhos. Lógica comum: os 3 métodos só diferem em **como obtêm a distribuição da estatística sob H0**.
- Paramétrico: fórmula (t, F, Normal); supõe normalidade e homocedasticidade.
- Permutação: embaralha os rótulos; supõe **intercambiabilidade**, ou seja, mesma distribuição e inclusive mesma variância sob H0.
- Bootstrap: reamostra com reposição, com F̂ no lugar de F; supõe uma amostra representativa.

**A. K > 2 grupos: lotes de coleta (base original).** Pergunta: o `clump_thickness` mudou ao longo dos 8 lotes? H0: μ₁ = … = μ₈.
- Paramétrico: ANOVA F ~ F(K−1, N−K).
- Permutação: Unid. 02, slides 18–19. Embaralha os 699 casos mantendo os nᵢ e calcula p = #(F* ≥ Fobs)/B, com B aleatório porque a enumeração é inviável.
- Bootstrap: translação ỹᵢⱼ = yᵢⱼ − ȳᵢ + ȳ (generalização dos slides 22 e 24), reamostrando **dentro** de cada lote. Tolera variâncias diferentes, ao contrário da permutação. Opção de usar o F de Welch e IC percentil para μ₁ − μ₈.
- Discussão de confundimento: a % de malignos varia de 16% a 45% entre os lotes, então convém refazer só com os benignos.
- Pontos fortes: justificativa forte para o não paramétrico (ordinal, nᵢ de 17 a 367) e alta originalidade. Ponto fraco: não há código da professora para esse cenário.

**B. 2 grupos independentes: benigno × maligno numa variável de pouca diferença (WDBC). PREFERIDA PELO USUÁRIO (19/10/2026).**
- Ex.: `fractal_dim_mean` (p de Welch = 0,77, assimetria 1,64 em B). H0: E(X) = E(Y), bilateral.
- Paramétrico: t de Welch.
- Permutação: Unid. 02, slides 11–16, com T1 = ΣXᵢ ou X̄ − Ȳ e B aleatório, já que C(569,357) é inviável. Valor-p bilateral = 2·min das caudas.
- Bootstrap: teste com translação para z̄ e estatística de Welch, valor-p com |T| (Unid. 03, slides 23–24). IC percentil para μM − μB reamostrando os originais (slide 18 e código das 2 Gamas).
- Bônus: teste de variâncias D3 = S²ₓ/S²ᵧ (slide 15; Lista 1, Ex2). É preciso centrar os grupos antes de permutar, porque o teste exige médias iguais. O F paramétrico de variâncias é muito sensível à falta de normalidade. Candidata: `symmetry_se`, com razão de variâncias 2,07.
- Ponto fraco: com n = 357/212 o TCL faz os 3 métodos concordarem. Contorno: **estudo com subamostras pequenas** (ex.: n = 10 por grupo, repetidas), para mostrar quando os métodos divergem.
- Variáveis com p **limítrofe** tendem a mostrar mais diferença entre os métodos no nível de 5%: `fractal_dim_se` (p = 0,042, assimetria 4,32 em B) e `smoothness_se` (p = 0,11).

**C. Regressão logística (original ou WDBC).** logit P(maligno) = β₀ + β₁x, OR = e^β₁, H0: β₁ = 0.
- Paramétrico: IC de Wald com EP da inversa da informação de Fisher (o EMV sai pelo escore de Fisher da Unid. 04) e IC do OR pelo método delta.
- Bootstrap dos **pares** (Unid. 03, slide 29). O bootstrap de resíduos não faz sentido com Y binário. Alternativa: bootstrap paramétrico com Y* ~ Bernoulli(p̂ᵢ).
- Permutação: embaralha y em relação a x.
- Risco: **separação**, em que β̂ → ∞ e algumas reamostras não convergem. É a abordagem mais trabalhosa.

**Escolha de B (vale para todas):** rodar B = 100, 500, 1000, 5000 e 10000, plotar o valor-p e os limites do IC em função de B e mostrar a estabilização. O erro de Monte Carlo do valor-p é ≈ √[p(1−p)/B].

Sugestão do agente: B é a mais simples e a mais próxima do código das aulas. Funciona bem **se** incluir o estudo com subamostras pequenas e/ou uma variável com p limítrofe. Sem isso, a conclusão vira "todos os métodos concordam porque n é grande". A combinação "A principal + B complemento" também foi sugerida.

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

**Lotes de coleta (base original):** o `.data` está em ordem cronológica, então o lote sai da posição da linha, com tamanhos 367, 70, 31, 17, 48, 49, 31 e 86 (conferido com `unformatted-data`). Proporção de malignos por lote: 0,455; 0,186; 0,290; 0,176; 0,250; 0,184; 0,452; 0,163. Mediana do `clump` por lote: 5, 3, 5, 5, 4, 4, 5, 3. **Datas dos lotes** (só mês/ano, que é a data de *montagem/envio* do lote, não a data de cada paciente): 1 = jan/1989, 2 = out/1989, 3 = fev/1990, 4 = abr/1990, 5 = ago/1990, 6 = dez/1990 (o `unformatted-data` diz "December 7, 1990; Updated Jan 8, 1991", e o `.names` diz "Updated January 1991"), 7 = jun/1991, 8 = nov/1991. Conferência linha a linha (19/10/2026): o `.data` dividido pelos tamanhos é **idêntico** ao `unformatted-data`, que tem outra ordem de colunas (a classe vem na 2ª coluna) e usa 0 no lugar de `?`. Então a atribuição do lote pela posição está confirmada. Divergências na documentação: o cabeçalho do grupo 7 diz 16B/15M, mas os dados têm 17B/14M; o rodapé diz "Current Total 698", mas são 699 linhas. Dos 46 ids repetidos, 16 aparecem em lotes diferentes (o `unformatted-data` cita um paciente que "reaparece no Grupo 8") e 30 dentro do mesmo lote.

**WDBC, variáveis com pouca diferença B×M** (d de Cohen, p de Welch): symmetry_se (−0,01; 0,89), texture_se (−0,02; 0,84), fractal_dim_mean (−0,03; 0,77; assimetria 1,64 em B), smoothness_se (−0,14; 0,11), fractal_dim_se (0,17; 0,04). Razão de variâncias M/B: symmetry_se 2,07, fractal_dim_worst 2,44.

**WDBC = dataset 17 do UCI** (https://archive.ics.uci.edu/dataset/17/breast+cancer+wisconsin+diagnostic). O `wdbc.data` que já está na pasta é essa base, então **não é preciso baixar de novo**. Conferido em 19/10/2026: é idêntico ao `sklearn.datasets.load_breast_cancer()` (569×30, mesmos valores e mesmo diagnóstico). O sklearn serve de referência para os nomes das colunas.

**Carregar a WDBC com Polars:** `wdbc.data` (sem cabeçalho). As colunas são `["id", "diagnostico"]` + `f"{c}_{estat}"` para `estat` em `["mean", "se", "worst"]` (nessa ordem, por fora) e `c` em `["radius", "texture", "perimeter", "area", "smoothness", "compactness", "concavity", "concave_points", "symmetry", "fractal_dim"]` (por dentro). Diagnóstico: B = benigno, M = maligno. Esses nomes foram dados pelo agente a partir do `wdbc.names`; o arquivo não traz nomes.

**O que é cada linha e cada coluna da WDBC** (explicado ao usuário em 19/10/2026):
- **Cada linha** é 1 paciente (1 massa mamária): uma punção aspirativa por agulha fina (FNA) foi digitalizada como imagem, e o software traçou o contorno de cada núcleo celular.
- **Colunas:** 10 características medidas em cada núcleo e resumidas por imagem de 3 formas: `_mean` (média dos núcleos), `_se` (erro padrão, ou seja, heterogeneidade entre os núcleos) e `_worst` (média dos 3 maiores valores).
- **Unidades:**
  - radius e perimeter: comprimento na escala da imagem. A unidade física **não** consta no `.names`; conferir em Street, Wolberg & Mangasarian (1993).
  - area: essa unidade ao quadrado. Coerente: radius_mean ≈ 14 e area_mean ≈ 655 ≈ π·14².
  - texture: DP dos tons de cinza.
  - smoothness, compactness, concavity, concave_points, symmetry e fractal_dim: grandezas **adimensionais**.

**OBRIGATÓRIO NOS SLIDES (pedido do usuário, 19/10/2026): explicar a estrutura dos dados**, nos itens 1 e 5 do checklist. A ideia a passar:
- 1 paciente → 1 **nódulo** (o tumor) → a agulha retira **algumas** células, que já são uma amostra do nódulo → imagem → vários **núcleos** (1 por célula) medidos pelo software.
- O arquivo **não** traz os núcleos individuais. Traz **1 linha por paciente**, com cada característica resumida de 3 formas: `_mean` (média das células), `_se` (o quanto as células variam entre si) e `_worst` (média das 3 maiores).
- O motivo: o diagnóstico é da paciente, não da célula.
- Deixar clara a diferença entre as **duas médias**: a média *dentro* da paciente, que já vem no arquivo (ex.: `fractal_dim_mean`), e a média *entre* as pacientes, que é o que testamos (benignas × malignas).
- Consequência estatística: cada linha é uma paciente diferente, então as observações são independentes, o que sustenta as suposições da permutação e do bootstrap. Se as linhas fossem núcleos, a independência cairia.
- Sugestão: um diagrama simples paciente → nódulo → células/núcleos → 1 linha. O texto dos slides é escrito pela dupla.

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
- Kernel Jupyter registrado como **"Python (MCI)"** (nome `mci`, em `%APPDATA%\jupyter\kernels\mci`).
- **Selecionar o kernel no VS Code:** se o VS Code estiver aberto em `VSCODE/`, o `.venv` do MCI **não aparece** automaticamente, porque a extensão só procura na raiz da pasta aberta. Há três saídas:
  - "Select Kernel" → "Select Another Kernel…" → "Jupyter Kernel…" → "Python (MCI)". Se não aparecer, rodar "Developer: Reload Window".
  - "Python Environments…" → "Enter interpreter path" → `C:\Users\Giovani Verotti\Desktop\VSCODE\MCI\.venv\Scripts\python.exe`.
  - Abrir `MCI/` com Open Folder. O usuário passou a fazer isso em 19/10/2026.
- Carregar a base original com Polars: `pl.read_csv(caminho, has_header=False, new_columns=COLUNAS, null_values="?", schema_overrides={"bare_nuclei": pl.Int64})`. O lote sai da posição da linha, com tamanhos `[367, 70, 31, 17, 48, 49, 31, 86]`. Classe: 2 = benigno, 4 = maligno. O usuário começou o notebook `notebooks/20261019_df_inicial.ipynb` com esse script.
- Recriar: `python -m venv .venv` → `.venv\Scripts\python -m pip install -r requirements.txt`.
- Ao instalar uma lib nova, atualizar o `requirements.txt` (`pip freeze > requirements.txt`, mantendo o cabeçalho) e anotar aqui.

## Convenções

- **Polars é obrigatório** para carregar e manipular dados (exigência do usuário, 19/10/2026). Não usar pandas. Conversão para numpy (`.to_numpy()`) só na hora de passar valores para scipy ou matplotlib.
- **Os scripts e notebooks precisam ser entendidos e reproduzidos à mão por um humano** (exigência do usuário). Isso significa:
  - células curtas, cada uma precedida por um markdown que explica o que faz e por quê, com a teoria quando houver;
  - nomes de variáveis em português;
  - nada de abstrações desnecessárias (funções só quando evitam repetição óbvia);
  - comentários onde o código não é autoexplicativo.
- Cores fixas nos gráficos: **azul `#2a78d6` = benigno** e **laranja `#eb6834` = maligno** (validadas para daltonismo). Figuras salvas em `figuras/<etapa>/`.
- Notebooks rodam com cwd = `MCI/notebooks/`, então a raiz é `Path.cwd().parent`. Para executar pela linha de comando, usar `MCI\.venv\Scripts\jupyter-nbconvert.exe --execute --ExecutePreprocessor.kernel_name=mci`. O `jupyter` do PATH é o do `.venv` de Tópicos em Redes.

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
- Dados baixados em `dados/breast+cancer+wisconsin+original/` (ainda fora do git, assim como `Material/`).
- Primeiro notebook: `notebooks/20261019_df_inicial.ipynb` (1 célula, polars). Ele lê o `.data` com `?` → nulo, cria a coluna `lote` (1–8) a partir dos tamanhos cronológicos e a coluna `diagnostico` (benigno/maligno). Usa caminho absoluto em `CAMINHO`, ainda não foi executado/salvo com saídas e ainda não trata os ids repetidos.

## Próximos passos

Plano em etapas (proposto e aceito em 19/10/2026); cada etapa = 1 notebook + figuras:
1. **Etapa 1, `notebooks/01_descritiva.ipynb`** (itens 2, 3, 4 e 7). **FEITA em 19/10/2026; falta a dupla escolher a variável** (célula 16 do notebook, "Decisão da dupla").
   - Conteúdo: carga e conferência; tabela de 10 pacientes; formato longo (`unpivot`); resumo por grupo; d de Cohen e razão de variâncias (ranking das 30); gráficos (histograma, boxplot e QQ padronizado) das 5 de menor |d|; Shapiro; atípicos pela regra 1,5·IQR; quadro final; perguntas-guia para a escolha.
   - Figuras em `figuras/01_descritiva/`.
   - Resultado das candidatas:

     | variável | d | razão var M/B | assimetria B / M | % atípicos B / M |
     |---|---|---|---|---|
     | symmetry_se | −0,013 | 2,07 | 1,37 / 2,50 | 2,8 / 7,5 |
     | texture_se | −0,018 | 0,67 | 1,65 / 1,53 | 3,4 / 3,3 |
     | fractal_dim_mean | −0,026 | 1,26 | 1,64 / 0,89 | 4,8 / 0,5 |
     | smoothness_se | −0,14 | 0,89 | 1,50 / 3,93 | 4,8 / 2,4 |
     | fractal_dim_se | 0,169 | 0,48 | 4,32 / 1,66 | 7,3 / 4,2 |

     Todas rejeitam a normalidade no Shapiro (p < 1e-5) e os QQ-plots são curvos (cauda direita longa).
2. **Etapa 2, `02_testes.ipynb`** (itens 5, 6 e 8): Welch, permutação e bootstrap (teste + IC). As funções ficam em `scripts/metodos.py`.
3. **Etapa 3, `03_escolha_B.ipynb`**: estabilização para B = 100 a 10000.
4. **Etapa 4, `04_subamostras.ipynb`**: n = 10, 20 e 30 por grupo, repetido; taxa de rejeição e discordância entre os métodos.
5. Etapa 5 (opcional): teste de variâncias.
6. Etapa 6: slides. O texto é do usuário; o agente revisa.

Também é preciso levantar as unidades de medida das variáveis da WDBC para o item 1 dos slides (o `.names` não informa; as medidas vêm de imagem digitalizada).
4. Montar o roteiro de análise seguindo o checklist de 8 itens + comparação paramétrico × permutação × bootstrap + escolha de B.

## Histórico

- **19/10/2026:** criação da pasta `MCI/`, `git init`, primeiro commit, ligação ao remote `gverotti/MCI` e push. Criação deste `AGENTS.md`. Criação do `.venv` + `requirements.txt` copiando o ambiente do Modelagem_Logistico. Leitura completa do `Material/` (Unidades 02–04 + códigos R). Leitura das Listas 1 e 2 e do enunciado do Trabalho 1. Usuário escolheu Python e a base Breast Cancer Wisconsin (Original); validação feita (ids repetidos, 16 faltantes, pasta contém também WDBC e WPBC). Propostas as abordagens A, B e C, com a teoria detalhada; o usuário preferiu a B. Script de leitura em Polars entregue; explicado por que o `.venv` não aparecia no seletor de kernel. Criado `CLAUDE.md` importando este arquivo, porque a memória do agente se perde ao abrir `MCI/` como pasta. Etapa 1 (`01_descritiva.ipynb`) criada e executada sem erros; definidas as convenções de Polars obrigatório e código legível por humanos.
