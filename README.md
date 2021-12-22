# Husky Example

Este proyecto se ha generado con [Angular CLI](https://github.com/angular/angular-cli) version 13.0.3. Abordaremos la instalación de husky
y la configuración del proyecto para usar git-hooks

## ¿Qué es un git-hook?

Son scripts que Git ejecutará antes o después de realizar acciones en tu repositorio, como crear un nuevo comit o subir código a un repositorio. Puedes obtener información sobre 
todos los git-hooks disponibles en la [documentación oficial](https://git-scm.com/docs/githooks).

## Uso y solución
Si el código no pasa una de las reglas de los Git hooks, no se realizará el commit y podrás ver un informe de error de lo que se debe corregir para que se pueda realizar el commit. Por otro lado, si el código pasa todas las reglas especificadas, el commit se creará automáticamente.

Si el código no pasa una de las reglas de los Git hooks, no se realizará el commit y podrás ver un informe de error de lo que se debe corregir para que se pueda realizar el commit. Por otro lado, si el código pasa todas las reglas especificadas, el commit se creará automáticamente.

## Instalación e inicialización de Husky

Instalamos husky:
`npm install husky --save-dev`

Ahora vamos a inicializarlo:
`npx husky-init`

## Configuración de los git-hooks

Por defecto cuando inicializas husky, te pone el git-hook de pre-commit con `npm test`. Vamos a quitarlo y por ahora lo dejaremos vacío.

Vamos a crear el commit pre-push con los test:

`npx husky add .husky/pre-push "npm test"`

Y ahora vamos a añadir el hook para commit lint:

`npx husky add .husky/commit-msg "npx --no-install commitlint --edit"`

## Instalación y configuración de commitlint

Instalamos commit-lint:

`npm install @commitlint/config-conventional @commitlint/cli --dev`

Vamos a declarar las reglas para los commits. Vamos a crear un fichero llamado .commitlintrc.json y le pondremos el siguiente código:

`echo {} > .commitlintrc.json`

```
{
    "extends": ["@commitlint/config-conventional"],
    "rules": {
        "type-enum": [2, "always", ["ci", "chore", "docs", "feat", "fix", "perf", "refactor", "revert", "style"]]
    }
}
```

Con esto para hacer un commit tendrá que seguir el estándar de [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/).

Un ejemplo:

`"fix(scope[optional]): your commit message"`

## Instalación y configuración eslint y prettier

Vamos a instalar eslint y prettier.

Instalamos prettier:
`npm install prettier`

Instalamos eslint:
`npm install eslint`

Ahora instalamos los plugins para typescript 
`npm i --save-dev @typescript-eslint/eslint-plugin`
`npm i --save-dev typescript @typescript-eslint/parser`

Vamos a crear el archivo .prettierc.json:

`echo {} > .prettierrc.json`

Ahora en el archivo podemos poner la configuración de prettier que deseemos:

```
{
  "singleQuote": true
}
```

También podemos incluir un archivo .prettierrcignore con los archivos o carpetas que queremos ignorar.

Por la parte de eslint, vamos a crear el fichero

`echo {} > .eslintrc.json`

Y ponemos lo siguiente (También se puede añadir configuración, os muestro un ejemplo básico)

```
{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": 11,
        "sourceType": "module"
    },
    "plugins": [
        "@typescript-eslint"
    ],
    "rules": {
      // aquí puedes añadir tus propias reglas
    }
}
```

## Instalación y configuración lint-staged

Vamos a instalar el plugin de lint-staged, este plugin hará que sobre el código que tengamos
en staged pendiente para hacer el commit se le aplique los pasos que especifiquemos (nosotros vamos a poner prettier y eslint).

`npm install lint-staged --save-dev`

Ahora en el package.json añadimos lo siguiente:

- En el apartado de scripts:

`"lint-staged": "lint-staged"`

- En un apartado nuevo (abajo de scripts, por ejemplo):

```
"scripts": {},
"lint-staged": {
  "**/**/*.{ts,css,json scss}": ["prettier --write", "eslint"] // puedes poner las extensiones que quieras
}
```

## Configuración pre-commit 

Ahora vamos al archivo de pre-commit (.husky/pre-commit) y añadimos el siguiente comando en el archivo:

`npm run lint:staged`

## Configuración de los test para pre-push

Antes pusimos en prepush los test. Ahora vamos a poner

Package.json -> scripts -> cambiamos el npm test por:
`"test": "ng test --watch=false",`

Ahora en el archivo karma.conf.js cambiamos lo siguiente en browsers:

`browsers: ['ChromeHeadless'],`

Con ChromeHeadless evitamos que se abra una instancia de chrome en los test, y aparezcan en la consola.

## ¡Listo para usarse!

Con esto tendrás una configuración bastante aceptable para añadir en tus proyectos y dar un salto más en la calidad y en el control del código que se va a subir. Espero que os sea de utilidad.

Si por alguna razón necesitar saltarte los git-hooks puedes añadir `--no-verify` al final del mensaje del commit.
