---
name: agil-incremental
description: Conduz um projeto de software a partir de uma ideia, em ciclos iterativos e incrementais guiados por rodadas de perguntas e respostas com o usuário, entregando software funcionando a cada iteração. Use ao iniciar um projeto novo a partir de uma ideia, ao continuar um projeto que tenha a pasta docs/agil/, ao planejar a próxima iteração ou ao revisar um incremento. Não use para correções pontuais em projetos que não seguem este framework.
---

# Ágil Incremental

## Princípio

Cada ciclo ataca primeiro a incerteza mais cara e termina com algo que roda, para que o feedback do usuário recaia sobre software real e não sobre suposições. Todas as regras abaixo derivam disso: as perguntas são escolhidas pelo risco, e nenhuma iteração fecha sem um incremento executável e verificado.

## Regras invioláveis

1. **Uma iteração é uma história de usuário**, fatiada na vertical (atravessa todas as camadas necessárias para rodar), com critérios de aceite escritos antes de qualquer código.
2. **Nenhuma iteração fecha sem software funcionando**: o incremento roda de ponta a ponta e cada critério de aceite tem evidência de que passou.
3. **Rodadas de 3 a 5 perguntas**, ordenadas por risco, cada uma com o motivo e uma resposta sugerida.
4. **Nada de decisão silenciosa.** Toda escolha relevante vai para `DECISOES.md`. Quando precisar supor algo para seguir, declare a suposição e registre.
5. **`DECISOES.md` só recebe acréscimos.** Mudar uma decisão é uma nova entrada que cita a anterior.
6. **`VISAO.md` só muda por decisão registrada.**
7. **O backlog é ordenado pelo agente e aprovado pelo usuário.** O agente propõe a ordem por risco; o usuário aprova ou reordena; a ordem aprovada é a que vale.

## Onde fica o estado

Todo o estado do projeto vive em `docs/agil/`, em Markdown puro, para que qualquer agente compatível com Agent Skills retome o trabalho de onde outro parou.

```
docs/agil/
  VISAO.md
  DECISOES.md
  BACKLOG.md
  iteracoes/
    ITERACAO-001.md
    ITERACAO-002.md
```

Os modelos estão em `templates/` nesta skill. Copie o modelo ao criar cada arquivo.

## Detecção de fase

Ao ser invocada, leia `docs/agil/` e aplique a primeira linha que for verdadeira:

| Condição | Fase |
|---|---|
| `VISAO.md` não existe | 0. Semente |
| Existe iteração com status `aguardando-aceite` | 5. Revisão |
| Existe iteração com status `em-execucao` ou `planejada` | 4. Execução |
| `BACKLOG.md` tem ordem com status `proposta` | 2. Aguardar aprovação do backlog |
| `VISAO.md` tem status `concluida` | Encerrado (informe e pare) |
| Qualquer outro caso | 1. Rodada de perguntas |

Diga ao usuário, em uma linha, em que fase o projeto está e o que vai acontecer a seguir.

## Fluxo contínuo com pausas

Avance de fase em fase sem pedir permissão. Pause, encerrando o turno, **somente** em quatro situações:

1. Ao fazer uma rodada de perguntas.
2. Ao propor a ordem do backlog para aprovação.
3. Ao pedir o aceite de uma iteração.
4. Diante de um bloqueio que exija decisão irreversível ou que contradiga a `VISAO.md`.

Fora disso, siga. Pausar sem necessidade é falha do fluxo.

## Fases

### 0. Semente

Faça uma rodada única cobrindo: a ideia em uma frase; o objetivo mensurável (como saber que deu certo); quem usa; o que fica explicitamente de fora; restrições conhecidas (plataforma, prazo, tecnologias obrigatórias ou proibidas).

Com as respostas, crie `VISAO.md`, abra `DECISOES.md` registrando a Semente como `D-001`, e escreva em `BACKLOG.md` as histórias candidatas no formato "Como [quem], quero [o quê], para [por quê]". Siga para a fase 1.

### 1. Rodada de perguntas

1. Liste as incertezas abertas sobre o produto e sobre as próximas histórias.
2. Classifique cada uma pelo custo de refazer caso a suposição esteja errada, multiplicado pela chance de estar errada.
3. Pergunte sobre as 3 a 5 de maior risco, no formato da seção "Formato das perguntas".
4. Pause.

Se não houver incerteza relevante aberta, diga isso e pule para a fase 2.

### 2. Registro e backlog

1. Registre cada resposta em `DECISOES.md`.
2. Atualize `BACKLOG.md`: histórias novas, alteradas ou descartadas.
3. Proponha a ordem por risco, com uma linha de justificativa por história, e marque a ordem como `proposta`.
4. Pause para aprovação ou reordenação.
5. Com a resposta, marque a ordem como `aprovada` (registrando em `DECISOES.md` se o usuário reordenou) e siga para a fase 3.

### 3. Fatia

Pegue a primeira história da ordem aprovada. Antes de planejar, verifique se ela cabe em uma iteração. Sinais de que não cabe: mais de cinco critérios de aceite, mais de um fluxo de usuário, ou dependência de algo que ainda não existe. Se não couber, quebre em histórias menores, devolva ao backlog e proponha a nova ordem (volta à pausa da fase 2).

Se couber, crie `iteracoes/ITERACAO-NNN.md` a partir do modelo, com critérios de aceite verificáveis e status `planejada`. Siga para a fase 4.

### 4. Execução

Mude o status para `em-execucao` e entregue a iteração ao framework de execução conforme a seção "Contrato com o framework de execução". Ao terminar, preencha as evidências de cada critério de aceite e mude o status para `aguardando-aceite`. Siga para a fase 5.

### 5. Revisão

Apresente ao usuário: como rodar o incremento, o que ele deve verificar e o resultado de cada critério de aceite com sua evidência. Pause pedindo o aceite.

- **Aceita:** status `aceita`, registre em `DECISOES.md` o que se aprendeu, marque a história como concluída no backlog. Se o objetivo da `VISAO.md` foi atingido, vá para o Encerramento; senão, volte à fase 1.
- **Recusada:** registre o motivo. Se o ajuste couber nos critérios atuais, volte à fase 4 na mesma iteração; se for uma necessidade nova, vire história no backlog e volte à fase 2.

### Encerramento

Quando o objetivo da `VISAO.md` for atingido ou o usuário encerrar, registre a decisão de encerramento em `DECISOES.md`, mude o status da `VISAO.md` para `concluida` e apresente um resumo: iterações entregues, o que ficou no backlog e por quê.

## Formato das perguntas

```
**1. [Título curto]**
[A pergunta, objetiva.]
Por que importa: [o que muda no produto conforme a resposta]
Sugestão: [resposta padrão recomendada e o motivo em uma frase]
```

A sugestão existe para que o usuário possa aceitar rápido quando não tiver opinião. Nunca faça perguntas cuja resposta já esteja em `VISAO.md` ou `DECISOES.md`.

## Contrato com o framework de execução

> **PENDENTE.** Esta seção será definida após a leitura do framework de vibe coding.
> Precisa responder: (1) em que formato a iteração é entregue ao executor (provavelmente o próprio `ITERACAO-NNN.md`); (2) o que o executor devolve para a Revisão (testes, diff, notas); (3) quem escreve e roda os testes de aceite.
>
> Até lá: implemente a história diretamente, escreva testes automatizados para os critérios de aceite sempre que possível, rode-os e registre o resultado como evidência.
