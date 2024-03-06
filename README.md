# Versionamento semântico

### Passos

#### Configurando validação de commits usando hooks

1. Instale o `commitlint` como dependência de desenvolvimento no seu projeto:

```bash
npm install --save-dev @commitlint/config-conventional @commitlint/cli
```

2. Instale o `husky` como dependência de desenvolvimento:

```bash
npm install --save-dev husky
```

3. Inicialize o `husky`:

```bash
npx husky init
```

4. Crie um arquivo de configuração do `commitlint` seguindo as configurações convencionais de commits semânticos:

```bash
echo "export default { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
```

5. Configure o hook `commit-msg` para executar o `commitlint`:

```bash
echo "npx --no -- commitlint --edit" > .husky/commit-msg
```

6. Teste um commit falso para verificar se as regras estão sendo aplicadas:

```bash
git commit -m "foo: bar" --allow-empty
```

O resultado deve ser algo como:
```bash
⧗   input: foo: bar
✖   type must be one of [build, chore, ci, docs, feat, fix, perf, refactor, revert, style, test] [type-enum]

✖   found 1 problems, 0 warnings
ⓘ   Get help: https://github.com/conventional-changelog/commitlint/#what-is-commitlint

husky - commit-msg script failed (code 1)
```

Isso configura o `commitlint` com o `husky` para garantir que suas mensagens de commit estejam em conformidade com as convenções especificadas. Certifique-se de ajustar os scripts conforme necessário, dependendo da estrutura do seu projeto.

[Referência](https://commitlint.js.org/guides/getting-started.html)
