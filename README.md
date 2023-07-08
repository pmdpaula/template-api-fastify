# Template para projetos de API com Fastify

## Configuração

Criar o arquivo `.npmrc` com o conteúdo a seguir para que os pacotes sejam instalados com a versão exata, oou seja, sem que seja possível atualizações indesejadas.

```
save-exact=true
```

- Adição do typescript

```bash
yarn add -D typescript @types/node tsx tsup

npx tsc --init
```

O segundo comando vai criar o arquivo `tsconfig.json` e podemos alterar a configuração do parâmetro `target` para `es2020`.

Com o tsx foi adicionado a linha a seguir nos scripts do `package.json`

```json
"start": "tsx watch src/main.ts",
```



- Lint e organização do código

```bash
yarn add -D eslint prettier eslint-config-prettier
```

[Ordenar as importações - eslint-plugin-import](https://github.com/import-js/eslint-plugin-import/)

```
yarn add -D eslint-plugin-import @typescript-eslint/parser eslint-import-resolver-typescript eslint-import-resolver-babel-module eslint-plugin-module-resolver @typescript-eslint/eslint-plugin
```

ver configurações necessárias do `eslint-import-resolver-typescript` para funcionar correto com o path mapping

[Ordenar as importações - @trivago/prettier-plugin-sort-imports](https://github.com/trivago/prettier-plugin-sort-imports#readme)

```bash
yarn add -D @trivago/prettier-plugin-sort-imports
```

Este plugin pede para que coloque a ordenação que queremos no arquivo `.prettierrc.json` e ficaram estas opções.

```json
  "importOrder": [
    "^react$",
    "^react-native$",
    "^@react-navigation$",
    "^@storage/(.*)$",
    "^@screens/(.*)$",
    "^@components/(.*)$",
    "^@assets/(.*)$",
    "^[./]"
  ],
  "importOrderSeparation": true,
  "importOrderSortSpecifiers": true
```

- [Ciar alias para os imports, evitando passar caminhos complexos - babel-plugin-module-resolver](https://github.com/tleunen/babel-plugin-module-resolver)
```
yarn add -D babel-plugin-module-resolver
```

### Path mapping

Mapeamento dos diretórios usando o babel-plugin-module-resolver.
Exemplo do que foi feito.

No arquivo `babel.config.js` foi adicionado as seguintes linhas:
```javascript
...
plugins: [
      ['module-resolver', {
        root: ['./src'],
        alias: {
          '@assets': './src/assets',
          '@components': './src/components',
          '@screens': './src/screens',
          '@types': './src/types',
        }
      }]
    ]

```

No arquivo `tsconfig.json` foi adicionado as seguintes linhas:
```javascript
...
"baseUrl": "./",
    "paths": {
      "@assets/*": ["./src/assets/*"],
      "@components/*": ["./src/components/*"],
      "@screens/*": ["./src/screens/*"],
      "@types/*": ["./src/@types/*"],
    }
```




- [Atualização de pacotes por bot do Github - renovate](https://docs.renovatebot.com/)

## Instalações

### Pacote da API

- [Framework web](https://fastify.dev/docs/latest/Guides/Getting-Started)

```bash
yarn add fastify
```



- [Variáveis de ambiente - dotenv](https://www.npmjs.com/package/dotenv)

```bash
yarn add dotenv
```

Será usado o arquivo `env/index.ts` para fazer as validações das variáveis de ambiente.



- [Validação de dados - zod](https://www.npmjs.com/package/zod)

```bash
yarn add zod
```



- [ORM (Object Relational Mapper) - Query builder - Prisma](https://www.prisma.io/docs/getting-started/quickstart)

```bash
yarn add -D prisma
```

>Inicia a configuração do prisma
>
>```bash
>npx prisma init
>```

Após a configuração modelo de dados, rodar o comando a seguir para gerar os códigos para manipular os modelos no banco.

```bash
npx prisma generate
```
-- Este comando também vai instalar o pacote `@prisma/client` que é o que vai ser usado para manipular os dados no banco.
Instalaria o `prisma` se já não tivesse instalado.

