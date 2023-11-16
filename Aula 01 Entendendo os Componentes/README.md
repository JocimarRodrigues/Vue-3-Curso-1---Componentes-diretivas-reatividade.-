# Para tu iniciar um projeto em vue o cmd é:
```js
npm create vue
//  Tu pode usar o @ para especificar uma versão específica do vue, exemplo: npm create vue@3.7.3
```

# Como é um componente em Vue

### No vue tu coloca tanto o template, quanto o css no mesmo componente, o nome disso é Single File Component

### Estrutura de um Componente

```vue
<template>
    <header class="banner">
        <div class="apresentacao">
            <img src="./assets/images/logo.svg" alt="Logo do Cookin Up" class="logo">
            <p class="cabecalho-lg frase-cabecalho">
                <span class="texto-verde">Um banquete de ideias para</span>
                despertar o chef que há em você!
            </p>
            <p class="subtitulo-lg">
                Explore novas receitas todos os dias com os ingredientes que estão ao seu alcance!
            </p>
        </div>
        <img src="./assets/images/foto-banner.png" alt="Foto de uma mulher cozinhando com uma bacia de vidro nas mãos."
            class="foto-banner">
    </header>
</template>

<style scoped>
.banner {
    padding: 4rem 7.5rem;
    color: var(--creme);

    display: flex;
    justify-content: space-between;
    align-items: center;
    column-gap: 3.25rem;
}

.logo {
    height: 4.5rem;
    margin-bottom: 3rem;
}

.frase-cabecalho {
    margin-bottom: 2rem;
}

.texto-verde {
    color: var(--verde-medio, #3D6D4A);
}

.foto-banner {
    width: 35rem;
}

@media only screen and (max-width: 1300px) {
    .banner {
        padding: 4rem 3.75rem;
        flex-direction: column;
        align-items: center;
        gap: 1rem;
    }

    .logo {
        display: block;
        margin-left: auto;
        margin-right: auto;
    }
}

@media only screen and (max-width: 767px) {
    .banner {
        padding: 4rem 1.5rem;
    }

    .foto-banner {
        width: min(100%, 21.25rem);
    }
}
</style>
```

- Note que é parecida com uma sintaxe de html vanilla, Tudo que estiver dentro do template é referente a html e dentro de style, é CSS

- O scoped é como se tu  estivesse usando o css modules, do react; isto é todo o style que está dentro desse componente não vai vazar para o outro, resumindo: Esse css só existe  nesse componente.

## O App.Vue é o responsável por  receber teus componentes e é dentro dele que tu vai importar e criar a lógica, tanto de import qnt export, segue o exemplo abaixo para entender melhor!

```vue
<script lang="ts"> // O lang é para tu especificar que a linguagem é TypeScript
import Banner from './components/Banner.vue'

export default {
  components: { Banner: Banner }
}
</script>

<template>
  <Banner />
</template>

```

- Isto vai funcionar de forma muito semelhante a como tu export e importa componentes em react, a diferenca aqui é que o import e  export do componentes é feito no App.Vue

- Nessa estrutura o App.vue é o componente Pai, e o Banner o componente filho.


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

