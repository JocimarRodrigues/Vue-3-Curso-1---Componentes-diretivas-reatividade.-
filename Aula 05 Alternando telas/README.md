# Controlando exibição de componentes

## Objetivo

- Tu quer criar uma lógica que mostre outro componente, no componente atual.

### Como fazer

- 1: Tu vai criar um novo Componente, no caso MostrarReceitas
MostrarReceitas.vue
```vue
<template>
    Mostrar receitas...
</template>
```
- 2: Tu vai criar um state, no Componente ConteudoPrincipal, tu vai usar esse state em um v-if, para mudar os componentes filho do ConteudoPrincipal

ConteudoPrincipal.vue
```vue
<script lang="ts">
import SelecionarIngredientes from './SelecionarIngredientes.vue';
import SuaLista from './SuaLista.vue';
import MostrarReceitas from './MostrarReceitas.vue'

type Pagina = 'SelecionarIngredientes' | 'MostrarReceitas'; // Aqui

export default {
  data() {
    return {
      ingredientes: ['Alho', 'Manteiga', 'Orégano'],
      conteudo: 'SelecionarIngredientes' as Pagina // Aqui
    };
  },
  components: { SelecionarIngredientes, SuaLista, MostrarReceitas },
  methods: {
    adicionarIngrediente(ingrediente: string) {
      this.ingredientes.push(ingrediente)
    },
    removerIngrediente(ingrediente: string) {
     this.ingredientes = this.ingredientes.filter(iLista => ingrediente !== iLista)
    }
  }
}
</script>

<template>
  <main class="conteudo-principal">
      <SuaLista :ingredientes="ingredientes" />

      <SelecionarIngredientes v-if="conteudo === 'SelecionarIngredientes'" @adicionar-ingrediente="adicionarIngrediente" @remover-ingrediente="removerIngrediente"/> <!--Aqui-->

      <MostrarReceitas  v-else-if="conteudo === 'MostrarReceitas'"/> <!--Aqui-->
  </main>
</template>
```

### O que o type Pagina está fazendo?

- Essa sintaxe que tu definiu, serviu para tipar o Pagina e especificar que ele só pode receber dois valores, q é o SelecionarIngredientes e MostrarReceitas, qualquer outro valor q tu passar para ele, irá ocorrer um erro. Isso é muito útil para evitar erros.

### Explicando o que está acontecendo

- Tu criou o state conteudo que vai que está recebendo um valor, no caso SelecionarIngredientes
- Tu tipo ele com o Type Pagina, especificando apenas 2 valores possíveis para esse state
- Depois tu criou uma diretiva condicional, usando esse state como parametro
- Dessa forma, o componente que vai ser redenrizado depende do parametro do state
- Note que essa forma está estática, mas claro tu pode utilizar uma lógica para deixar ela dinâmica.

## Deixando o evento dinâmico, utilizando o button para trocar o componente

### Como tu vai fazer isso?

- 1: Tu vai ir no componente que esta o button, q no caso é SelececionarIngredientes
- 2: Lá dentro tu vai criar um v:on:emit no button
SelecionarIngrediente.vue
```vue
<script lang="ts">
import { obterCategorias } from '@/http/index'
import type ICategoria from '@/interfaces/ICategoria'
import CardCategoria from './CardCategoria.vue'
import BotaoPrincipal from './BotaoPrincipal.vue';

export default {
    data() {
        return {
            categorias: [] as ICategoria[]
        };
    },
    async created() {
        this.categorias = await obterCategorias();
    },
    components: { CardCategoria, BotaoPrincipal },
    emits: ['adicionarIngrediente', 'removerIngrediente', 'buscarReceitas'] // Não esquece de adicionar o nome do emit, aqui
}
</script>

<template>
    <section class="selecionar-ingredientes">
        <h1 class="cabecalho titulo-ingredientes">Ingredientes</h1>

        <p class="paragrafo-lg instrucoes">Selecione abaixo os ingredientes que você quer usar nesta receita:</p>

        <ul class="categorias">
            <li v-for="categoria in categorias" :key="categoria.nome">
                <CardCategoria :categoria="categoria" @adicionar-ingrediente="$emit('adicionarIngrediente', $event)"
                    @remover-ingrediente="
                        $emit('removerIngrediente', $event)" />
            </li>
        </ul>

        <p class="paragrafo dica">
            *Atenção: consideramos que você tem em casa sal, pimenta e água.
        </p>

        <BotaoPrincipal texto="Buscar receitas!" @click="$emit('buscarReceitas')"/> <!--Aqui-->
    </section>
</template>
```

- 3: Agora que tu já criou o evento de click, basta voltar no componente de ConteudoPrincipal, e no componente de SelecionarIngredientes, tu usar o emit para mudar o valor do state conteudo

ConteudoPrincipal.vue
```vue
<template>
  <main class="conteudo-principal">
    <SuaLista :ingredientes="ingredientes" />

    <SelecionarIngredientes v-if="conteudo === 'SelecionarIngredientes'" @adicionar-ingrediente="adicionarIngrediente"
      @remover-ingrediente="removerIngrediente" @buscar-receitas="conteudo = 'MostrarReceitas'"/>

    <MostrarReceitas v-else-if="conteudo === 'MostrarReceitas'" /> <!--Aqui-->
  </main>
</template>
```

- Dessa forma quando tu clicar no botao, o state vai receber o valor MostrarReceitas, assim realizando a condicao para mudar o Componente e mostrar o componente MostrarReceitas

### Refatorando

- Em vez de tu aplicar a lógica de mudar o state, dentro do v-on, tu pode criar uma função para isso; é uma boa prática.

ConteudoPrincipal.vue
```vue
<script lang="ts">
import SelecionarIngredientes from './SelecionarIngredientes.vue';
import SuaLista from './SuaLista.vue';
import MostrarReceitas from './MostrarReceitas.vue'

type Pagina = 'SelecionarIngredientes' | 'MostrarReceitas';

export default {
  data() {
    return {
      ingredientes: ['Alho', 'Manteiga', 'Orégano'],
      conteudo: 'SelecionarIngredientes' as Pagina
    };
  },
  components: { SelecionarIngredientes, SuaLista, MostrarReceitas },
  methods: {
    adicionarIngrediente(ingrediente: string) {
      this.ingredientes.push(ingrediente)
    },
    removerIngrediente(ingrediente: string) {
      this.ingredientes = this.ingredientes.filter(iLista => ingrediente !== iLista)
    },
    navegar(pagina: Pagina) { // Aqui
      this.conteudo = pagina;
    }
  }
}
</script>

<template>
  <main class="conteudo-principal">
    <SuaLista :ingredientes="ingredientes" />

    <SelecionarIngredientes v-if="conteudo === 'SelecionarIngredientes'" @adicionar-ingrediente="adicionarIngrediente"
      @remover-ingrediente="removerIngrediente" @buscar-receitas="navegar('MostrarReceitas')"/> <!--Aqui-->

    <MostrarReceitas v-else-if="conteudo === 'MostrarReceitas'" />
  </main>
</template>
```

# Mantendo um componente vivo

- Sabe quando tu passa uma prop para um componente, e depois vai para outro? O Componente anterior é removido da tela  e caso tu volte/re-rendize esse componente, ele vai perder os state, certo?
- Então, o vue tem um componente que IMPEDE isso, mesmo q o componente q tenha os state seja removido,  quando tu voltar para ele os states ainda vão estar nele.

### O Componente que vai fazer isso é o KeepAlive

Exemplo
ConteudoPrincipal.vue
```vue
<template>
  <main class="conteudo-principal">
    <SuaLista :ingredientes="ingredientes" />

    <KeepAlive>

      <SelecionarIngredientes v-if="conteudo === 'SelecionarIngredientes'" @adicionar-ingrediente="adicionarIngrediente"
        @remover-ingrediente="removerIngrediente" @buscar-receitas="navegar('MostrarReceitas')" />

      <MostrarReceitas v-else-if="conteudo === 'MostrarReceitas'" @editar-receitas="navegar('SelecionarIngredientes')" />

    </KeepAlive>

  </main>
</template>
```

- O que esse componente faz é guardar esses componentes no cache, e quando tu pedir para renderizar novamente, ele só vai buscar na memória e renderizar novamente, mantendo os state.

### Como selecionar os componentes que o KeepAlive, vai guardar no cache

- Para fazer isso, tu vai atribuir um atributo chamado include="nome do componente"
- Note também q tu precisa ir no Componente que tu quer armazenar e dentro do script dele, tu adicionar uma propriedade chamada name="nome do componente"

Exemplo
ConteudoPrincipal.vue
```vue
<template>
  <main class="conteudo-principal">
    <SuaLista :ingredientes="ingredientes" />

    <KeepAlive include="SelecionarIngredientes"> <!--Aqui-->

      <SelecionarIngredientes v-if="conteudo === 'SelecionarIngredientes'" @adicionar-ingrediente="adicionarIngrediente"
        @remover-ingrediente="removerIngrediente" @buscar-receitas="navegar('MostrarReceitas')" />

      <MostrarReceitas v-else-if="conteudo === 'MostrarReceitas'" @editar-receitas="navegar('SelecionarIngredientes')" />

    </KeepAlive>

  </main>
</template>
```

SelecionarIngredientes.vue
```vue
<script lang="ts">
import { obterCategorias } from '@/http/index'
import type ICategoria from '@/interfaces/ICategoria'
import CardCategoria from './CardCategoria.vue'
import BotaoPrincipal from './BotaoPrincipal.vue';

export default {
    name: 'SelecionarIngredientes', // Aqui
    data() {
        return {
            categorias: [] as ICategoria[]
        };
    },
    async created() {
        this.categorias = await obterCategorias();
    },
    components: { CardCategoria, BotaoPrincipal },
    emits: ['adicionarIngrediente', 'removerIngrediente', 'buscarReceitas']
}
</script>
```

- Fazendo dessa forma o KeepAlive só vai armazenar no cache o  Componente SelecionarIngredientes, mesmo que o MostrarReceitas também esteja dentro dele.