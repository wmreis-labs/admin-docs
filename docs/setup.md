# Setup

O projeto foi iniciado com Create React App.

```bash
npm install
npm start
```

`npm test` abre o runner. `npm run build` gera a pasta `build`.

Toda chamada autenticada sai de `src/services/api/baseApi.ts`, com `Authorization` e `x-tenant`, na base `REACT_APP_API_URL`. Variáveis de ambiente ficam no repositório privado e não são republicadas aqui.

Autenticação e tenant são o domínio Auth do [airtestto](https://wmreis-labs.github.io/airtestto-docs/dominios/auth/).
