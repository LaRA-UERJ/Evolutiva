# Evolutiva

Avaliação continuada assistida por IA, com devolutiva individual ao aluno, contestação fundamentada e revisão do professor.

O nome é o programa: a escala de notas (avaliar) e escalar (evoluir). A devolutiva que produz evolução.

## O problema

- A prova única é a **única** evidência de aprendizagem. Mede no fim, quando não há mais tempo de corrigir rota para o aluno.
- Avaliação continuada em papel exige correção constante, e o custo é proibitivo: professores relatam em média **9,9 h/semana** corrigindo ([Learnosity, 258 professores](https://www.edutopia.org/article/why-teachers-should-grade-less-frequently/)), e a correção aparece associada a **mais emoção negativa que pesquisa ou aula** ([Studies in Higher Education, 2024](https://www.tandfonline.com/doi/full/10.1080/03075079.2023.2267533)).
- O resultado é o pior dos dois mundos: poucas avaliações, feedback que chega tarde, e nenhuma informação sobre quem está travado em quê.

## O que o sistema faz

1. **Captura** — a folha respondida à mão é fotografada ou escaneada (foto de celular basta). PDF com várias páginas também entra.
2. **Leitura** — um modelo de visão lê a folha e **transcreve fielmente**, preservando os erros do aluno e marcando `[ilegivel]` onde não deu. Folha em branco nunca é "corrigida".
3. **Correção por critério** — o modelo recebe o **gabarito escrito pelo professor** + a lista de critérios, e devolve pontos por critério com justificativa e o trecho exato da resposta que sustenta cada decisão. Nota global nunca é pedida.
4. **Rodadas independentes** — a mesma folha é corrigida 3 vezes, com vieses diferentes (neutro, rigoroso, detalhado). Onde as rodadas concordam, o item está resolvido. **Onde divergem, o item vai para o professor** — é aí que está o sinal de confiança, e ele sai de graça.
5. **Devolutiva ao aluno** — o aluno recebe a imagem da própria folha, a transcrição que a IA leu, a nota provisória, e a justificativa critério por critério com o trecho citado.
6. **Contestação** — um quadro na devolutiva onde o aluno marca *concordo / não concordo* e escreve o motivo por extenso. Uma rodada, com prazo.
7. **Revisão do professor** — o professor olha **só** o que foi contestado, o que divergiu entre rodadas ou o que saiu com confiança baixa. O resto segue.

A garantia central: o professor revisa uma minoria dos julgamentos, e sabe com precisão por que cada item da fila está lá.

## Decisões de projeto (não são arbitrárias)

- **Imagem direto no modelo de visão. Sem OCR intermediário.** Transcrever manuscrito para texto antes de corrigir é gargalo documentado: o erro de transcrição contamina a nota ([Perš et al., CVWW 2026](https://cmp.felk.cvut.cz/cvww2026/assets/pdfs/CVWW2026-45-final.pdf)).
- **A transcrição aparece para o aluno.** Se a IA leu errado, o erro fica visível antes de qualquer discussão sobre nota. É o primeiro item a contestar.
- **Gabarito de referência é obrigatório.** Sem solução de referência e com prompt simples, o pipeline **superavalia sistematicamente** — dá ponto onde não há ([CVWW 2026](https://cmp.felk.cvut.cz/cvww2026/assets/pdfs/CVWW2026-45-final.pdf)).
- **Múltiplas rodadas com vieses diferentes, não a mesma pergunta repetida.** Rodadas idênticas em temperatura 0 não carregam informação: a divergência só é sinal se houver diversidade real. E o viés rigoroso ataca diretamente o modo de falha conhecido (generosidade).
- **Validação determinística.** O modelo propõe os números; o script recalcula somas, corta pontos acima do máximo e mapeia critérios aos nomes do gabarito. Nada de aceitar JSON de modelo como verdade.
- **Nota por critério, com o trecho citado.** É o que torna a devolutiva discutível em vez de arbitrária.
- **Rodar local.** A folha de um aluno é dado pessoal (nome + caligrafia + desempenho). O modelo roda no Ollama da rede interna; nada sai daqui.

### Evidência de que o modelo é suficiente para o trabalho

- VLM corrigindo prova manuscrita por critério: **QWK 0,727** contra o professor, enquanto **dois professores humanos entre si deram 0,551** — ou seja, a IA concordou mais com o professor do que os humanos concordam entre si (1.982 registros de critério, 20 configurações, [arXiv 2609.14284](https://arxiv.org/pdf/2609.14284)).
- O mesmo estudo mostra o limite: em 5 rodadas sobre o mesmo material, **50 a 64% dos critérios mudaram de nota**. Rodada única é instável — daí a agregação por consenso.
- Pipeline completo em prova de engenharia com diagrama desenhado à mão: **diferença média de ~8 pontos** e **taxa de revisão manual abaixo de 20%** ([CVWW 2026](https://cmp.felk.cvut.cz/cvww2026/assets/pdfs/CVWW2026-45-final.pdf)).
- Few-shot prompting **piorou** o resultado em todas as configurações testadas — o ganho está na estrutura do prompt e na referência, não em exemplos.
- Gerar as próprias questões é atividade de estudo com efeito positivo, mas a evidência é contestada (o efeito pode vir do aluno mais aplicado que participa, [ACM 2012](https://dl.acm.org/doi/10.1145/2157136.2157250)). Reportado como hipótese, não como fato.

## Estado

Desenho e esqueleto de configuração. O protótipo do pipeline existe localmente e **ainda não foi executado contra o modelo nem validado com folha real**. Nenhum número deste repositório foi medido neste sistema — todos vêm das referências acima.

## Como rodar (protótipo)

```bash
cp config/avaliacao.exemplo.toml config/avaliacao.toml   # editar disciplina e questões
cp config/gabarito.exemplo.md   config/gabarito.md       # escrever o gabarito
mkdir -p entrada                                          # pôr as folhas (PDF ou foto)
python3 corrigir.py                                       # -> saida/
```

Saídas: `saida/devolutivas/<aluno>.html` (e `.pdf`), `saida/notas.csv`, `saida/revisao.md` (a fila do professor).

## Privacidade

`entrada/` e `saida/` estão no `.gitignore` e nunca são versionados. Gabarito e configuração da disciplina em uso também ficam fora do repositório — só os arquivos de exemplo são versionados.

## Referências

- Tonmoy et al., *Vision-Language Models for Criterion-Level Grading of Handwritten Examinations in Outcome-Based Education*, IEEE TLT — https://arxiv.org/pdf/2609.14284
- Perš et al., *Grading Handwritten Engineering Exams with Multimodal Large Language Models*, CVWW 2026 — https://cmp.felk.cvut.cz/cvww2026/assets/pdfs/CVWW2026-45-final.pdf
- Singh et al., *Gradescope: a fast, flexible, and fair system for scalable assessment of handwritten work*, L@S 2017 — https://dl.acm.org/doi/10.1145/3051457.3051466
- *Evaluating large language models for AI-assisted grading*, Scientific Reports 2026 — https://www.nature.com/articles/s41598-026-48656-3
