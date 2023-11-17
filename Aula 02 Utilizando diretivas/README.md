# Sobre Diretivas

-  Uma diretiva do Vue é basicamente um atributo que sempre começa com v-.

ConteudoPrincipal.vue
```vue
<script lang="ts">
export default {}
</script>

```

- Aí quando tu for no App.vue tu vai ter o auto complete do vscode, para esse componente
App.vue

```vue
<script lang="ts">
import Banner from './components/Banner.vue'
import ConteudoPrincipal from './components/ConteudoPrincipal.vue';

export default {
  components: { Banner: Banner, ConteudoPrincipal }
}
</script>

<template>
  <Banner />
  <ConteudoPrincipal />
</template>



```

# V For

### O V for serve para tu não precisar repetir código html no Vue, ele funciona basicamente como um for do JavaScript, olhe o exemplo abaixo

Com o V-For
```vue
<template>
    <main class="conteudo-principal">
        <section>
            <span class="subtitulo-lg sua-lista-texto">Sua lista:</span>
            <ul class="ingredientes-sua-lista">
                <li v-for="ingrediente in ['Alho', 'Manteiga', 'Orégano']" class="ingrediente">
                    {{ ingrediente }} 
                </li>
            </ul>
        </section>
    </main>
</template>
```

Sem o V-For

```vue
<template>
    <main class="conteudo-principal">
        <section>
            <span class="subtitulo-lg sua-lista-texto">Sua lista:</span>
            <ul class="ingredientes-sua-lista">
                <li class="ingrediente">
                    Alho
                </li>
                <li class="ingrediente">
                    Manteiga
                </li>
                <li class="ingrediente">
                    Óregano
                </li>
            </ul>
        </section>
    </main>
</template>
```

### Notar que a forma acima não é a mais correta para alcançar esse resultado, a forma mais correta é tu criar um objeto dentro do script do componente e usar esse objeto no v-for

## Exemplo

- Lembrar que essa lógica precisa ser criada DENTRO do exprot default do componente

```vue
<script lang="ts">
export default {
    data() {
        return {
            ingredientes: ['Alho', 'Manteiga', 'Orégano']
        }
    }
}
</script>

```

### Aí refatorando o template, ficaria assim

```vue
<template>
    <main class="conteudo-principal">
        <section>
            <span class="subtitulo-lg sua-lista-texto">Sua lista:</span>
            <ul class="ingredientes-sua-lista">
                <li v-for="ingrediente in ingredientes" class="ingrediente">
                    {{ ingrediente }} 
                </li>
            </ul>
        </section>
    </main>
</template>
```

- Essa é a forma mais correta de se usar o v-for.

# V-bind

- O V-bind serve para tu pegar o "nome" do atributo de um v-for, para tu usar no key, de forma grosseira, ele pega o nome do item do for, para usar na key.

### Exemplo

```vue
<template>
    <main class="conteudo-principal">
        <section>
            <span class="subtitulo-lg sua-lista-texto">Sua lista:</span>
            <ul class="ingredientes-sua-lista">
                <li v-for="ingrediente in ingredientes" v-bind:key="ingrediente" class="ingrediente">
                    {{ ingrediente }} 
                </li>
            </ul>
        </section>
    </main>
</template>
```

## Tem um atalho forma q é muito utilizado para usar o v-bind. Q em vez de tu escrever v-bind=key="value" tu escreve :key="value"

### Exemplo

```vue

<template>
    <main class="conteudo-principal">
        <section>
            <span class="subtitulo-lg sua-lista-texto">Sua lista:</span>
            <ul class="ingredientes-sua-lista">
                <li v-for="ingrediente in ingredientes" :key="ingrediente" class="ingrediente">
                    {{ ingrediente }} 
                </li>
            </ul>
        </section>
    </main>
</template>
```

# Renderizando condicionalmente

### Tu pode usar a diretiva v-if para colocar uma condicao em uma tag, para redenrizar ela condicionalmente

Exemplo

```vue
<template>
    <main class="conteudo-principal">
        <section>
            <span class="subtitulo-lg sua-lista-texto">Sua lista:</span>
            <ul v-if="ingredientes.length" class="ingredientes-sua-lista"> <!--Aqui-->
                <li v-for="ingrediente in ingredientes" :key="ingrediente" class="ingrediente">
                    {{ ingrediente }} 
                </li>
            </ul>
        </section>
    </main>
</template>
```

- Note que essa ul, só vai ser renderizada se o array ingredientes existir

### Aí para tu renderizar algo, caso o if, esteja ativo, tu usa o v-else

Exemplo

```vue
<template>
    <main class="conteudo-principal">
        <section>
            <span class="subtitulo-lg sua-lista-texto">Sua lista:</span>
            <ul v-if="ingredientes.length" class="ingredientes-sua-lista">
                <li v-for="ingrediente in ingredientes" :key="ingrediente" class="ingrediente">
                    {{ ingrediente }} 
                </li>
            </ul>

            <p class="paragrafo lista-vazia" v-else>  <!--Aqui-->
              <img src="../assets/images/icones/lista-vazia.svg" alt="Ícone de pesquisa">
              Sua lista está vazia, selecione ingredientes para iniciar.
            </p>
        </section>
    </main>
</template>
```

-  Note qe caso o conteúdo de ingredientes, for vazio, o parágrafo vai ser renderizado com o uso do v-else

#### Importante lembrar que o v-else tem q ser utilizado logo depois do v-if, caso isso não seja respeitado o v-else não irá funcionar!




### Módulo 07/aula 02

# Outra forma de se obter os dados

- Essa forma é muito importante, porque ela vai ser bem semelhante para quando tu for pegar os dados vindos de uma API!!!

- Para fazer isso, tu vai criar um arquivo e dentro dele tu vai criar e exportar uma função

Exemplo

```ts
//http/index.ts
export function obterCategoria() {
    return [
        [
            {
                "nome": "Laticínios e Ovos",
                "ingredientes": ["Ovos", "Queijo", "Leite", "Manteiga", "Creme de Leite", "Iogurte", "Leite Condensado", "Sorvete"],
                "rotulo": "laticinios_e_ovos"
            },
            {
                "nome": "Farinhas e Fermentos",
                "ingredientes": ["Farinha de trigo", "Polvilho", "Farinha de rosca", "Canjica", "Farinha de mandioca", "Fubá", "Linhaça", "Fermento químico"],
                "rotulo": "farinhas_e_fermentos"
            }
        ]
    ]
}


```

## Agora dentro do Componente que tu quer usar esses dados, dentro do export default dele, tu vai passar essa função

```vue
<script lang="ts">
import {obterCategorias} from '@/http/index'

export default {
    data() {
        return {
            categorias: obterCategorias()

        }
    }
}
</script>
```
- O @ serve para definir o caminho base como o src

- Note que o @ do import é uma peculiadade do  vite, mas para isso funcionar e não dar erro no vscode, tu vai precisar definir isso no tsconfig.app.json, na pasta raíz

```json
{

    "include": ["env.d.ts", "src/**/*", "src/**/*.vue", "src/**/*.ts"],
}
```

- Tu adicionou o último src

# Fazendo uma requisição http

- Tu vai usar uma funcao async para usar o fetch e enviar a resposta

```ts
//http/index.ts
export async function obterCategorias() {
  const resposta = await fetch(
    "https://gist.githubusercontent.com/antonio-evaldo/002ad55e1cf01ef3fc6ee4feb9152964/raw/bf463b47860043da3b3604ca60cffc3ad1ba9865/categorias.json"
  );

  const categorias = await resposta.json();

  return categorias;
}

```

- Agora como tu tá usando uma função async, tu não vai usar ela dentro do data()
- Para isso existe um método chamado created() que é criado depois do data()
- Isso faz parte do ciclo de  vida de um componente do vue

Exemplo

```vue
<script lang="ts">
import { obterCategorias } from '@/http/index'

export default {
    data() {
        return {
            categorias: []

        }
    },
    async created() {
        this.categorias = await obterCategorias()
    }
}
</script>
```

### Explicando o que está acontecendo

- Você cria a função async para fazer os protocolos http
- Dentro do componente tu cria o data() que é o responsável por receber os dados, ele vai ser criado ANTES do created()
- Como o created() é criado depois do data(), tu vai poder usar as propriedades do data()
- Dessa forma tu consegue definir para o categorias receber os dados que estão vindo do json lá do teu get do http


### Observação interessante

- Todas as propriedades dentro do data, são estados, reativas
- Isso faz com que toda vez que o valor dela seja alterado, o componente é re-renderizado.

# Tipando o http

- É bem semelhante ao ts, tu cria a interface, importa ela e define na variável que tu quer.

Exemplo

```ts
//src/interfaces/ICategoria.ts
export default interface Icategoria {
    nome: string;
    ingredientes: string[];
    imagem: string;
}
```

```ts
//http/index.ts
import type ICategoria from "@/interfaces/ICategoria";

export async function obterCategorias() {
  const resposta = await fetch(
    "https://gist.githubusercontent.com/antonio-evaldo/002ad55e1cf01ef3fc6ee4feb9152964/raw/bf463b47860043da3b3604ca60cffc3ad1ba9865/categorias.json"
  );

  const categorias: ICategoria[] = await resposta.json();

  return categorias;
}

```

### Tu também precisausar a interface, dentro do Data()

```ts
//src/components/SelecionarIngredientes.vue
<script lang="ts">
import { obterCategorias } from '@/http/index'
import type ICategoria from '@/interfaces/ICategoria'

export default {
    data() {
        return {
            categorias: [] as ICategoria[]

        }
    },
    async created() {
        this.categorias = await obterCategorias()
    }
}
</script>
```
