# Avaliação GitHub Actions com IA

## Tarefa 1 — CI

- Execução verde:

https://github.com/Cereal-F/avaliacao-github-actions/actions/runs/38651079534

- Execução vermelha:

https://github.com/Cereal-F/avaliacao-github-actions/actions/runs/38602728427

- Linha do log que mostra o erro:

```text
Error: Possível segredo no repositório
```

- Por que testar a migração em SQLite antes do D1:

O Cloudflare D1 é baseado em SQLite. Testar as migrações primeiro em SQLite permite validar a sintaxe SQL, a estrutura das tabelas e possíveis erros antes da execução no banco remoto, reduzindo riscos e facilitando a correção de problemas.

---

## Tarefa 2 — Proteção da main

- PR bloqueado:

https://github.com/Cereal-F/avaliacao-github-actions/pull/8

- PR liberado:

https://github.com/Cereal-F/avaliacao-github-actions/pull/8

- Push direto recusado:

```text
You can't commit to main because it is a protected branch
```

---

## Tarefa 3 — Revisão por IA

- PRs com comentários da IA:

https://github.com/Cereal-F/avaliacao-github-actions/pull/7

https://github.com/Cereal-F/avaliacao-github-actions/pull/8

- Execução com erro de permissão:

https://github.com/Cereal-F/avaliacao-github-actions/actions/runs/38603814846

- Afirmação incorreta ou irrelevante da IA:

```text
Nenhum problema crítico encontrado.
```

- Por que a afirmação está errada ou é irrelevante:

O comentário gerado foi genérico e não analisou detalhadamente as alterações do Pull Request. A mensagem não apresentou problemas específicos do código modificado, demonstrando que a revisão por IA deve ser utilizada como apoio ao desenvolvedor e não como substituta de uma revisão técnica humana.

---

## Tarefa 4 — Migrações D1

- Execução da migração:

https://github.com/Cereal-F/avaliacao-github-actions/actions/runs/38651079735

- Resultado do PRAGMA:

Comando executado:

```sql
PRAGMA table_info(notas);
```

Resultado:

```json
[
  {
    "cid": 0,
    "name": "id",
    "type": "INTEGER",
    "pk": 1
  },
  {
    "cid": 1,
    "name": "aluno",
    "type": "TEXT",
    "notnull": 1
  },
  {
    "cid": 2,
    "name": "email",
    "type": "TEXT"
  }
]
```

A saída confirma que a coluna `email` foi criada com sucesso na tabela `notas`.

- Resultado da migração:

```text
0001_inicial.sql ✅
0002_teste.sql ✅
0003_pragmateste.sql ✅
```

Trecho do log:

```text
🌀 Executing on remote database oauth-sessions-pedro

0001_inicial.sql ✅
0002_teste.sql ✅
0003_pragmateste.sql ✅
```

- Merge sem alteração em migrations/:

https://github.com/Cereal-F/avaliacao-github-actions/pull/11

O PR alterou apenas o arquivo AVALIACAO.md. Como não houve alterações na pasta migrations/, o workflow migrar não foi executado. Isso confirma que o filtro:

paths:
  - 'migrations/**'

está funcionando corretamente.

Observação:

O workflow foi configurado com:

```yaml
paths:
  - 'migrations/**'
```

Portanto, ele só executa q**ndo existem alterações na pasta `**grations`.

---

## Perguntas

##**1.

O arquivo `ci.yml` executa du** vezes porque está associado aos **entos `push` e `pull_request`. Qu**do existe um Pull Request aberto **ocorre um push na branch correspo**ente, ambos os eventos são dispar**os. O status check considerado pe** proteção da branch principal é o**tatus gerado para o Pull Request,**ois é ele que precisa ser aprovad**para permitir o merge.

### 2.

P**l Requests vindos de forks não re**bem acesso aos secrets do reposit**io por questões de segurança. Ess**regra impede que um usuário malic**so modifique um workflow para exi**r ou transmitir tokens, senhas e **tras credenciais armazenadas nos **crets do projeto.

### 3.

O work**ow declara permissões explicitame**e para aplicar o princípio do men** privilégio. Dessa forma, o `GITH**_TOKEN` recebe apenas os acessos **cessários para executar a tarefa,**eduzindo riscos de segurança e li**tando os impactos de configuraçõe**incorretas.

### 4.

Não. A revis** por IA pode auxiliar na identificação de problemas, mas não garante que todos os erros sejam encontrados. Durante a atividade foi observado que a IA produziu comentários genéricos, como "Nenhum problema crítico encontrado", demonstrando que ela pode deixar passar problemas ou produzir análises pouco detalhadas. Por isso, a revisão por IA complementa, mas não substitui os status checks obrigatórios e a revisão humana.

---

## Bônus — Resumo por IA no GITHUB_STEP_SUMMARY

Implementado no workflow `revisao-ia`.

Execução:

https://github.com/Cereal-F/avaliacao-github-actions/actions/runs/38661793226

Conteúdo exibido na aba Summary:

```text
Resumo da Revisão por IA

A revisão automática foi executada com sucesso.

Principais verificações realizadas:

- Estrutura do repositório
- Alterações do Pull Request
- Pipeline GitHub Actions
- Arquivos modificados

Nenhum problema crítico encontrado.
```

O resumo foi gerado automaticamente utilizando a variável
`$GITHUB_STEP_SUMMARY` durante a execução do workflow.
