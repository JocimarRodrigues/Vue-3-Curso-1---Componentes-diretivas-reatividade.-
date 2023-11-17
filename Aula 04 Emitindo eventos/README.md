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