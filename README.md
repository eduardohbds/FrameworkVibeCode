# Instalação

A skill segue o padrão aberto Agent Skills. Mantenha uma única cópia em `.agents/skills/` e aponte o Claude Code para ela com um link simbólico.

## Por projeto (recomendado)

Na raiz do repositório:

```bash
mkdir -p .agents/skills .claude/skills
cp -r /caminho/para/agil-incremental .agents/skills/
ln -s ../../.agents/skills/agil-incremental .claude/skills/agil-incremental
```

## Em todos os projetos

```bash
mkdir -p ~/.agents/skills ~/.claude/skills
cp -r /caminho/para/agil-incremental ~/.agents/skills/
ln -s ~/.agents/skills/agil-incremental ~/.claude/skills/agil-incremental
```

## Onde cada agente procura

- Claude Code (principal): `.claude/skills/`
- Codex (reserva): `.agents/skills/`
- Gemini CLI (reserva): `.gemini/skills/` ou `.agents/skills/`

Os caminhos mudam com frequência; confira a documentação de cada ferramenta ao instalar.

## Uso

Peça ao agente para começar um projeto a partir de uma ideia, ou para continuar um projeto que já tenha `docs/agil/`. A skill descobre a fase sozinha lendo os arquivos.
