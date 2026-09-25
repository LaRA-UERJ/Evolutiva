# Gabarito oficial

Escreva aqui em portugues, como voce explicaria para o corretor. Este texto e o unico
conhecimento que o modelo tem sobre a sua prova: ele nao pode conceder ponto que nao esteja
previsto aqui.

## Q1 - Equacao geral do plano por tres pontos

Esperado:

1. Vetores diretores: AB = B - A = (1, 1, -1) e AC = C - A = (-1, 2, 1).
2. Vetor normal: n = AB x AC = (3, 0, 3). Aceitar qualquer multiplo escalar nao nulo.
   Aceitar tambem n = (-3, 0, -3) e versoes normalizadas.
3. Equacao geral: 3x + 0y + 3z = 9, ou equivalentemente x + z = 3.
   Conferir com os tres pontos: A(1,0,2) -> 1+2 = 3 ok. B(2,1,1) -> 2+1 = 3 ok.
   C(0,2,3) -> 0+3 = 3 ok.

Pontuacao parcial:
- Errou o determinante mas acertou os vetores diretores: pontua o criterio 1, zera o 2 e o 3.
- Achou o normal certo mas errou a montagem da equacao: pontua 1 e 2, zera o 3.
- Testou os tres pontos na equacao final para conferir: mencionar em observacao (nao da ponto extra).

## Q2 - Area do triangulo

Esperado:

- Area = |AB x AC| / 2 = |(3,0,3)| / 2 = sqrt(18)/2 = (3 sqrt(2))/2, aproximadamente 2,12.

Aceitar sqrt(18)/2 sem simplificar, ou 2,12 com duas casas.
Aceitar caminho por produto escalar (|u|^2 |v|^2 - (u.v)^2) se chegar ao mesmo valor.
Nao aceitar |AB x AC| sem dividir por 2 (o criterio "valor numerico correto" zera, o
criterio "usou area = ..." zera tambem, porque a formula escrita nao e a da area).

## Q3 - Produto vetorial de vetores paralelos

Esperado (duas linhas de raciocinio validas, qualquer uma serve):

- Via angulo: |u x v| = |u||v| sen(theta). Paralelos -> theta = 0 ou 180 -> sen(theta) = 0
  -> |u x v| = 0 -> u x v = 0.
- Via dependencia linear: u = k v. Produto vetorial de um vetor com um multiplo escalar de si
  mesmo e nulo, porque as componentes se cancelam duas a duas no determinante.

Nao aceitar como justificativa apenas "porque da zero" ou "porque sao iguais": a resposta
precisa nomear o mecanismo (angulo nulo ou dependencia linear).

## Regras gerais de correcao

- Resposta certa por caminho diferente do gabarito: pontuar integralmente e registrar em observacao.
- Unidade ou notacao diferente mas consistente: nao penalizar.
- Calculo errado que nao compromete o raciocinio: nao penalizar duas vezes pelo mesmo erro
  (ex.: se o produto vetorial esta errado, nao zerar tambem a equacao que depende dele se o
  metodo de montagem estiver correto).
