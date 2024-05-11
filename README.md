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

Isso configura o [commitlint](2) com o [husky](3) para garantir que suas mensagens de commit estejam em conformidade com as convenções especificadas. Certifique-se de ajustar os scripts conforme necessário, dependendo da estrutura do seu projeto.

#### Configurando o versionamento
7. Versionando as releases de forma automáticas:
  - 7.1 Instale [standard-version](1) como dependência de desenvolvimento
  ```bash
  npm install --save-dev standard-version
  ```

  - 7.2 Crie em seu package.json o seguinte comando no objeto de scripts
  ```json
    "release": "standard-version"
  ```

  - 7.3 Ao executar o comando `npm run release` os seguintes passos são efetuado:
    - Análise dos Commits;
    - Incremento da Versão do projeto package.json com base nos commits;
    - Geração do CHANGELOG.md com informações sobre a versão, mudanças feitas;

  - 7.4 Agora é enviar as mudanças para o repositório
  ```bash
  git push --follow-tags origin main.
  ```
  - 7.5 Lembre-se que pode criar um hook para perguntar no momento do push se o usuário deseja gerar uma nova versão ou não do proejeto, como por exemplo um pre-push:
  ```bash
    #!/bin/sh

    echo "> iniciando pre push :)"
    exec < /dev/tty

    while true; do
      read -p "[pre-push hook] Deseja realisar o versionamento do projeto? (s/n)" sn
      if [ "$sn" = "" ]; then
        sn='s'
      fi
      case $sn in
        [Ss] ) npm run release; break;;
        [Nn] ) exit;;
        * ) echo "Por favor responda s ou n para Sim ou não.";;
      esac
  done

  ```

Feito essas configurações teremos mais qualidade em nossas releases e trabalhando de forma padronizada.

[Referência](2)

[1]: https://github.com/conventional-changelog/standard-version
[2]: https://commitlint.js.org/guides/getting-started.html
[3]: https://typicode.github.io/husky/
