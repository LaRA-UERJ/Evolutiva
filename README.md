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

A prova já existe estruturada: **sempre quatro questões com vários itens**, e o gabarito já é escrito pelo professor. O revisor digital compara com o gabarito que já existe — não inventa critério próprio.

São três passadas, e a ordem importa.

1. **Leitura** — o modelo de visão transcreve cada folha e extrai, item por item, o caminho de resolução que o aluno usou. Nesta passada ele **não** julga se está certo.

2. **Colheita de caminhos alternativos** — antes de corrigir qualquer coisa, o sistema lista os caminhos que chegaram ao resultado por rota diferente da do gabarito, mostrando o trabalho do aluno. O professor decide, um a um, quais são válidos, e o gabarito é incrementado.

   É aqui que o custo cai. O julgamento caro do professor passa a ser gasto **por caminho distinto**, não por aluno: se doze alunos usam a mesma solução alternativa, é uma decisão só, e depois a correção trata os doze igual.

3. **Correção final** — com o gabarito já enriquecido, cada folha é corrigida item por item contra ele, em rodadas independentes. Onde as rodadas concordam, o item está resolvido; onde divergem, o item vai para o professor.

4. **Devolutiva ao aluno** — o que ele fez, a correção por item, os argumentos, a nota, e o espaço para concordar ou discordar.

5. **Revisão do professor** — o professor olha o que foi contestado e o que ficou em dúvida. O resto segue.

### Por que a comparação com o gabarito reduz a alucinação

A tarefa do modelo deixa de ser "resolver a questão" e passa a ser "ler o que o aluno escreveu e comparar com o gabarito". Ele não precisa saber matemática melhor que o professor; precisa ler e comparar. Essa é uma tarefa muito mais restrita, e é o que torna o erro de correção improvável em vez de improvável por sorte.

### O limite, e onde ele morde

Na colheita, sem o gabarito na frente, o modelo pode racionalizar uma resposta **errada** como caminho alternativo válido. Por isso ele não opina nessa passada: apenas apresenta a rota e o trabalho do aluno, e **quem declara válido é o professor**.

O erro inverso é pior. Um caminho aceito sem exame contamina a turma inteira na passada seguinte. Essa é a passada que merece atenção do professor; a correção final, não.

### Consequência estrutural

Com questões de vários itens, a rubrica passa a ser **por item**, não por questão.

Nada aqui foi validado na prática ainda. É o desenho a ser testado.

## O que ainda não está decidido

1. Como o aluno recebe a devolutiva, e como a contestação volta para o professor.
2. Se a nota da máquina vale direto ou só depois da revisão do professor.
3. Quantas avaliações por semestre e quanto vale cada uma na média da disciplina.
4. Se a atividade é o aluno **responder** questões ou o aluno **escrever** as questões.
5. O que fazer com folha ilegível.
6. Qual modelo, e onde ele roda.
7. Quantas folhas por vez e quanto tempo leva para uma turma.
8. Quantos caminhos alternativos aparecem numa turma real — é o que decide se a passada de colheita é barata ou se vira trabalho.
9. **Onde cortar o gatilho de revisão.** É o botão que troca erro por trabalho: quanto mais apertado, mais folha cai no colo do professor e menos erro passa. Este desenho usa a discordância entre rodadas no nível do item, mas o valor de corte ainda não foi escolhido.
10. **O que a contestação do aluno cobre e o que ela não cobre.** Ela pega o erro contra o aluno. Nota alta indevida ninguém contesta — e o viés documentado é justamente o positivo. Falta definir a auditoria do outro lado.

## Evidência considerada

Números de terceiros, não medidos neste sistema:

- Correção de prova manuscrita por critério com modelo de visão: **QWK 0,727** contra o professor, enquanto **dois professores humanos entre si deram 0,551** (1.982 registros de critério, 20 configurações).
- O mesmo estudo: em 5 rodadas sobre o mesmo material, **50 a 64% dos critérios mudaram de nota**. Correção de rodada única é instável — daí as rodadas repetidas no passo 3.
- Pipeline completo em prova de engenharia com diagrama desenhado à mão: **diferença média de ~8 pontos numa escala de 0 a 100** — ou seja, 8% da prova. Esse número vale junto com o gatilho de revisão: **menos de 20% das folhas revisadas** quando o gatilho é uma discordância de 40 pontos (de 100) entre os corretores.
- O gatilho deles é a discordância **entre os corretores da própria máquina**, e foi medido no nível da prova inteira. O artigo diz explicitamente que aplicar o mesmo critério no nível da questão ou do item reduz a quantidade de intervenção humana — que é o que este desenho faz. E a taxa nunca chega a zero: existem folhas em que os corretores discordam sempre.
- O modo de falha documentado é o **viés positivo**: um dos modelos testados apresentou "superavaliação sistemática", e basta tirar as regras e o gabarito do prompt para que o viés positivo apareça nos dois melhores modelos.

- 258 professores dos EUA relataram **9,9 h por semana** corrigindo (levantamento da Learnosity), e em amostra de docentes universitários a correção aparece associada a mais emoção negativa que pesquisa ou aula (Schwab et al., *Studies in Higher Education*, 2024) — ambas as citações conforme o levantamento do CVWW 2026.

## Estado

Desenho. O pipeline ainda **não está neste repositório** e não foi executado. Os arquivos em `config/` são exemplos do formato de entrada pretendido (questões, critérios com pesos e gabarito em prosa).

## Privacidade

Folha de aluno é dado pessoal: nome, caligrafia e desempenho. `entrada/` e `saida/` estão no `.gitignore` e nunca são versionados; o gabarito da prova em uso também fica fora do repositório.

## Referências

- Tonmoy et al., *Vision-Language Models for Criterion-Level Grading of Handwritten Examinations in Outcome-Based Education* — https://arxiv.org/pdf/2609.14284
- Perš et al., *Grading Handwritten Engineering Exams with Multimodal Large Language Models*, CVWW 2026 — https://cmp.felk.cvut.cz/cvww2026/assets/pdfs/CVWW2026-45-final.pdf
- *Evaluating large language models for AI-assisted grading*, Scientific Reports 2026 — https://www.nature.com/articles/s41598-026-48656-3
