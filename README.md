# Para tu iniciar um projeto em vue o cmd é:
```js
npm create vue
//  Tu pode usar o @ para especificar uma versão específica do vue, exemplo: npm create vue@3.7.3
```

# Observações

### Uma dica legal, é o comando tree para tu criar uma estrutura da pasta em um formato texto

- Tem que usar o comando tree na pasta raíz do projeto para tu montar a estrutura, segue um exemplo do resultado

```
C:.
├───.vscode
├───node_modules
│   ├───.bin
│   ├───.vite
│   │   └───deps
│   ├───@babel
│   │   └───parser
│   │       ├───bin
│   │       ├───lib
│   │       └───typings
│   ├───@esbuild
│   │   └───win32-x64
│   ├───@jridgewell
│   │   └───sourcemap-codec
│   │       └───dist
│   │           └───types
│   ├───@vitejs
│   │   └───plugin-vue
│   │       └───dist
│   ├───@vue
│   │   ├───compiler-core
│   │   │   └───dist
│   │   ├───compiler-dom
│   │   │   └───dist
│   │   ├───compiler-sfc
│   │   │   └───dist
│   │   ├───compiler-ssr
│   │   │   └───dist
│   │   ├───reactivity
│   │   │   └───dist
│   │   ├───reactivity-transform
│   │   │   └───dist
│   │   ├───runtime-core
│   │   │   └───dist
│   │   ├───runtime-dom
│   │   │   └───dist
│   │   ├───server-renderer
│   │   │   └───dist
│   │   └───shared
│   │       └───dist
│   ├───csstype
│   ├───esbuild
│   │   ├───bin
│   │   └───lib
│   ├───estree-walker
│   │   ├───dist
│   │   │   ├───esm
│   │   │   └───umd
│   │   ├───src
│   │   └───types
│   ├───magic-string
│   │   └───dist
│   ├───nanoid
│   │   ├───async
│   │   ├───bin
│   │   ├───non-secure
│   │   └───url-alphabet
│   ├───picocolors
│   ├───postcss
│   │   └───lib
│   ├───rollup
│   │   └───dist
│   │       ├───bin
│   │       ├───es
│   │       │   └───shared
│   │       └───shared
│   ├───source-map-js
│   │   └───lib
│   ├───vite
│   │   ├───bin
│   │   ├───dist
│   │   │   ├───client
│   │   │   ├───node
│   │   │   │   └───chunks
│   │   │   └───node-cjs
│   │   └───types
│   └───vue
│       ├───compiler-sfc
│       ├───dist
│       ├───jsx-runtime
│       └───server-renderer
├───public
└───src
    ├───assets
    └───components
        └───icons
PS C:\Programming\Vu
```

### Para mostrar os arquivos dentro da pasta, tu usa o cmd tree /f, isso no windows.

Exemplo

```
├───public
│       favicon.ico
│
└───src
    │   App.vue
    │   main.js
    │
    ├───assets
    │       base.css
    │       logo.svg
    │       main.css
    │
    └───components
        │   HelloWorld.vue
        │   TheWelcome.vue
        │   WelcomeItem.vue
        │
        └───icons
                IconCommunity.vue
                IconDocumentation.vue
                IconEcosystem.vue
                IconSupport.vue
                IconTooling.vue
```

