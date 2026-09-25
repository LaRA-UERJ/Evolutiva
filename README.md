# Evolutiva

Avaliação continuada assistida por IA. O aluno vê o que fez, a correção, os argumentos, e diz se concorda ou não; o professor revisa depois.

## Objetivo

**Dar mais avaliações ao longo do semestre.**

Hoje são poucas. A prova acabou virando a única evidência de aprendizagem, o que é ruim: mede o resultado no fim, quando já não há tempo de o aluno corrigir rota. Avaliar mais vezes, e devolver rápido, melhora para o aluno.

O obstáculo é direto e não tem rodeio: **cada avaliação a mais é correção a mais**, e isso vira sobrecarga para o professor. Esse é o conflito que este repositório existe para resolver — não há outro objetivo aqui.

A saída é reduzir o custo de correção **por avaliação**, sem tirar o professor da decisão. A máquina lê a folha, aplica o gabarito escrito pelo professor e produz uma primeira passada com os argumentos de cada decisão. O professor julga o que a máquina não resolveu sozinha e dá a palavra final. É correção assistida; a nota continua sendo do professor.

## O que o aluno deve receber

- o que ele fez
- a correção da IA
- os argumentos
- a chance de dizer se concorda ou não

O professor olha depois.

## O problema, nas palavras do professor

> "Os alunos não prestam atenção, não estudam em casa. Tenho feito umas folhas para eles fazerem atividade em sala, mas ainda mantenho a prova. E isso é ruim. Eu tenho que fazer uma avaliação continuada. Mas dá muito trabalho."

## Desenho proposto (proposta, não decisão tomada)

1. **Captura** — a folha respondida à mão é fotografada ou escaneada. Foto de celular basta.
2. **Leitura** — um modelo de visão lê a folha e transcreve, marcando o que não conseguiu ler.
3. **Correção por critério** — o modelo recebe o gabarito escrito pelo professor e devolve pontos por critério, com a justificativa de cada um.
4. **Rodadas independentes** — a mesma folha é corrigida mais de uma vez. Onde as correções concordam, o item está resolvido; onde divergem, o item vai para o professor.
5. **Devolutiva ao aluno** — o que ele fez, a correção por critério, os argumentos, a nota, e o espaço para concordar ou discordar.
6. **Revisão do professor** — o professor olha o que foi contestado e o que ficou em dúvida. O resto segue.

Nada aqui foi validado na prática ainda. É o desenho a ser testado.

## O que ainda não está decidido

1. Como o aluno recebe a devolutiva, e como a contestação volta para o professor.
2. Se a nota da máquina vale direto ou só depois da revisão do professor.
3. Quantas avaliações por semestre e quanto vale cada uma.
4. O que fazer com folha ilegível.
5. Qual modelo e onde ele roda.

## Evidência considerada

Números de terceiros, não medidos neste sistema:

- Correção de prova manuscrita por critério com modelo de visão: **QWK 0,727** contra o professor, enquanto **dois professores humanos entre si deram 0,551** (1.982 registros de critério, 20 configurações).
- O mesmo estudo: em 5 rodadas sobre o mesmo material, **50 a 64% dos critérios mudaram de nota**. Correção de rodada única é instável — daí o passo 4.
- Pipeline completo em prova de engenharia com diagrama desenhado à mão: **diferença média de ~8 pontos** e **revisão humana acionada em menos de 20% dos casos**.
- Sem solução de referência, e com prompt simples, o pipeline **superavalia sistematicamente**.
- 258 professores dos EUA relataram **9,9 h por semana** corrigindo (levantamento da Learnosity), e em amostra de docentes universitários a correção aparece associada a mais emoção negativa que pesquisa ou aula (Schwab et al., *Studies in Higher Education*, 2024) — ambas as citações conforme o levantamento do CVWW 2026.

## Estado

Desenho. O pipeline ainda **não está neste repositório** e não foi executado. Os arquivos em `config/` são exemplos do formato de entrada pretendido (questões, critérios com pesos e gabarito em prosa).

## Privacidade

Folha de aluno é dado pessoal: nome, caligrafia e desempenho. `entrada/` e `saida/` estão no `.gitignore` e nunca são versionados; o gabarito da prova em uso também fica fora do repositório.

## Referências

- Tonmoy et al., *Vision-Language Models for Criterion-Level Grading of Handwritten Examinations in Outcome-Based Education* — https://arxiv.org/pdf/2609.14284
- Perš et al., *Grading Handwritten Engineering Exams with Multimodal Large Language Models*, CVWW 2026 — https://cmp.felk.cvut.cz/cvww2026/assets/pdfs/CVWW2026-45-final.pdf
- *Evaluating large language models for AI-assisted grading*, Scientific Reports 2026 — https://www.nature.com/articles/s41598-026-48656-3
