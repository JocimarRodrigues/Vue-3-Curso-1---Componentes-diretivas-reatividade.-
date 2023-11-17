# Controlando  visual com estado

- A lógica que tu está tentando criar é fazer a condicional de classe, funcionar com um evento de clique, para cada vez que tu clicar em um ingrediente, ele mudar de cor/classe
- Para fazer isso, primeiro tu vai criar um novo Componente, chamado IngredienteSelecionavel e dentro dele tu vai colocar o Componente Tag

IngredienteSelecionavel.vue
```vue
<script lang="ts">
import Tag from './Tag.vue';

export default {
    components: { Tag },
    props: {
        ingrediente: { type: String, required: true }
    }
}

</script>

<template>
    <Tag :texto="ingrediente" />
</template>
```

- Aí dentro do CardCategoria, tu vai chamar esse componente e passar a props de ingrediente para ele, até aqui o resultado é o mesmo que tu tinha antes, ainda está estático

CardCategoria.vue
```vue
<template>
    <article class="categoria">
        <header class="categoria__cabecalho">
            <img :src="`/imagens/icones/categorias_ingredientes/${categoria.imagem}`" alt="" class="categoria__imagem">

            <h2 class="paragrafo-lg categoria__nome">{{ categoria.nome }}</h2>
        </header>

        <ul class="categoria__ingredientes">
            <li v-for="ingrediente in categoria.ingredientes" :key="ingrediente">
       
                <IngredienteSelecionavel :ingrediente="ingrediente"/>
            </li>
        </ul>
    </article>
</template>
```

### Adicionado o evento

- Tu vai criar um state, dentro do componente de IngredienteSelecionavel

IngredienteSelecionado.vue
```vue
<script lang="ts">
import Tag from './Tag.vue';

export default {
    components: { Tag },
    props: {
        ingrediente: { type: String, required: true }
    },
    data() { // Aqui
        return {
            selecionado: false
        }
    }
}

</script>
```

- Agora tu vai fazer com que a props "ativa", do componente Tag, que tu criou na aula anterior, receba esse state.

IngredienteSelecionado.vue
```vue
<template>
    <Tag :texto="ingrediente" :ativa="selecionado"/>  <!--Aqui-->
</template>
```

- Depois disso, tu vai envolver o componente em um button e usar a diretiva v-on
- Essa diretiva, permite escutar eventos do DOM, no caso tu vai selecionar para escutar o evento de click
- Depois basta tu colocar a instrução do javascript q tu quer fazer, arrow function funciona?

Exemplo

IngredienteSelecionado.vue
```vue
<template>
    <button class="ingrediente" v-on:click="selecionado = !selecionado">

        <Tag :texto="ingrediente" :ativa="selecionado" />
    </button>
</template>
```

- Dessa forma, tu conseguiu criar um comportamento no teu código, cada vez q tu clicar nesse button a prop ativa, vai receber o contráio dela = caso for true, vai receber false e vice versa

### Existe um atalho para a diretiva v:on q é o @, então em vez de digitar v-on:click="", basta digitar @click=""


IngredienteSelecionado.vue
```vue
<template>
    <button class="ingrediente" @click="selecionado = !selecionado" :aria-pressed="selecionado">

        <Tag :texto="ingrediente" :ativa="selecionado" />
    </button>
</template>
```

## Boa prática :aria-pressed

- Usar o atributo aried-pressed para indicar q o botão está pressionado, não é obrigatório, mas é uma boa prática.

# Usando funcoes no v:on

- Para usar funcoes no v:on, basta tu criar um novo método, chamado methods, no export default e lá dentro criar a função


IngredienteSelecionado.vue
```vue
<script lang="ts">
import Tag from './Tag.vue';

export default {
    components: { Tag },
    props: {
        ingrediente: { type: String, required: true }
    },
    data() {
        return {
            selecionado: false
        }
    },
    methods: {
        aoClicar() {
            this.selecionado = !this.selecionado
        }
    }
}

</script>

<template>
    <button class="ingrediente" v-on:click="aoClicar" :aria-pressed="selecionado">

        <Tag :texto="ingrediente" :ativa="selecionado" />
    </button>
</template>

```

- Esse código tem o mesmo resultado que o de baixo, a diferenca é q está usando uma função.

IngredienteSelecionado.vue
```vue
<script lang="ts">
import Tag from './Tag.vue';

export default {
    components: { Tag },
    props: {
        ingrediente: { type: String, required: true }
    },
    data() {
        return {
            selecionado: false
        }
    },
}

</script>

<template>
    <button class="ingrediente" v-on:click="selecionado = !selecionado" :aria-pressed="selecionado">

        <Tag :texto="ingrediente" :ativa="selecionado" />
    </button>
</template>
```

# Passando um state para o componente pai

### Para fazer isso tu vai usar  o emit

- O emit é uma função especial do vue, para criar um evento personalizado
- No primeiro parametro é o nome do evento, o 2 é o dado q o evento vai carregar

IngredienteSelecionavel.vue
```vue
<script lang="ts">
import Tag from './Tag.vue';

export default {
    components: { Tag },
    props: {
        ingrediente: { type: String, required: true }
    },
    data() {
        return {
            selecionado: false
        }
    },
    methods: {
        aoClicar() {
            this.selecionado = !this.selecionado

            if(this.selecionado) {
                this.$emit('adicionarIngrediente', this.ingrediente)
            }
        }
    },
    emits: ['adicionarIngrediente']
}

</script>
```

### Emits

- É uma boa prática, serve para tu definir um array com o nome dos eventos personalizados que o componente é capaz de emitir

## Consumindo o emit

- Tu vai ir no componente pai e dentro dos atributos do componente q está emitindo o evento tu vai usar o v-on tu vai colocar a ação que tu quer utilizar

- No caso, tu quer subir esse componente mais vezes, para fazer isso, basta tu definir na ação tu vai usar o emit, para subir ele novamente

- No emit o primeiro parametro, vai ser o nome do evento, e o 2 tu vai digitar $event para continuar mandando o dado q tá vindo do emit do componente filho

- Lembrar também q tu também precisar adicionar o emits dentro desse componente.

CardCategoria.vue
```vue
<script lang="ts">
import type Icategoria from '@/interfaces/ICategoria';
import type { PropType } from 'vue';
import Tag from './Tag.vue';
import IngredienteSelecionavel from './IngredienteSelecionavel.vue';

export default {
    props: {
        categoria: { type: Object as PropType<Icategoria>, required: true }
    },
    components: { Tag, IngredienteSelecionavel },
    emits: ['adicionarIngrediente'] // Aqui
}
</script>

<template>
    <article class="categoria">
        <header class="categoria__cabecalho">
            <img :src="`/imagens/icones/categorias_ingredientes/${categoria.imagem}`" alt="" class="categoria__imagem">

            <h2 class="paragrafo-lg categoria__nome">{{ categoria.nome }}</h2>
        </header>

        <ul class="categoria__ingredientes">
            <li v-for="ingrediente in categoria.ingredientes" :key="ingrediente">

                <IngredienteSelecionavel :ingrediente="ingrediente"
                    @adicionar-ingrediente="$emit('adicionarIngrediente', $event)" /> <!--Aqui-->
            </li>
        </ul>
    </article>
</template>
```

- Dessa forma tu consegue ficar subindo o componente até o componente que tu quer realizar a lógica

### Realizando a lógica no componente, utilizando o evento personalizado

- Quando tu chegar no componente que tu quer realizar a lógica, dentro do v-on, basta tu fazer a lógica.

ConteudoPrincipal.vue
```vue
<script lang="ts">
import SelecionarIngredientes from './SelecionarIngredientes.vue';
import Tag from './Tag.vue';
import SuaLista from './SuaLista.vue';

export default {
  data() {
    return {
      ingredientes: ['Alho', 'Manteiga', 'Orégano']
    };
  },
  components: { SelecionarIngredientes, SuaLista }
}
</script>


<template>
  <main class="conteudo-principal">
      <SuaLista :ingredientes="ingredientes" />

      <SelecionarIngredientes @adicionar-ingrediente="ingredientes.push($event)"/> <!--Aqui-->
  </main>
</template>
```

- O que está acontecendo aí, é que você está adicionado ao array ingredientes, o ingrediente q tu tá pegando lá do componente IngredienteSelecionado.
- Note que o valor do $event tá vindo do evento personalizado/emit

### Refatorando

- Tu pode usar uma funcao no v-on em vez de fazer a lógica toda alí, é mais organizado
ConteudoPricipal.vue
```vue
<script lang="ts">
import SelecionarIngredientes from './SelecionarIngredientes.vue';
import Tag from './Tag.vue';
import SuaLista from './SuaLista.vue';

export default {
  data() {
    return {
      ingredientes: ['Alho', 'Manteiga', 'Orégano']
    };
  },
  components: { SelecionarIngredientes, SuaLista },
  methods: {
    adicionarIngrediente(ingrediente: string) { // Aqui
      this.ingredientes.push(ingrediente)
    }
  }
}
</script>

<template>
  <main class="conteudo-principal">
      <SuaLista :ingredientes="ingredientes" />

      <SelecionarIngredientes @adicionar-ingrediente="adicionarIngrediente"/> <!--Aqui-->
  </main>
</template>
```