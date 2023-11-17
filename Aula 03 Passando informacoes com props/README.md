# Como passar props

- Tu vai passar o componente que vai receber a props e nos atributos dele, tu vai usar o v-bind, para passar a props

Exemplo


```vue
<!--SelecionarIngredientes.vie-->
<template>
    <section class="selecionar-ingredientes">
        <h1 class="cabecalho titulo-ingredientes">Ingredientes</h1>

        <p class="paragrafo-lg instrucoes">Selecione abaixo os ingredientes que você quer usar nesta receita:</p>

        <ul class="categorias">
            <li v-for="categoria in categorias" :key="categoria.nome">
                <CardCategoria :categoria="categoria"/> <!--Aqui-->
            </li>
        </ul>

        <p class="paragrafo dica">
            *Atenção: consideramos que você tem em casa sal, pimenta e água.
        </p>
    </section>
</template>
```

- Dentro do Componente filho tu vai dentro do script, definir props: passar a props q ele está recebendo e tipar ela

Exemplo

```vue
<script lang="ts">
    export default {
        props: {
            categoria: Object // Aqui
        }
    }
</script>

<template>
    {{ categoria.nome }}
</template>
```

- Note que tu está definindo que a props categoria é um Objeto, porque oq está sendo passado é um Objeto com várias propriedades, nome, imagem, etc

#### Mas dessa forma aí, o ts vai reclamar, dizendo que possívelmente a props pode ser undefined, para arrumar isso, tu tem q definir q o type dela é um Object e usar um required para dizer que essa é uma props obrigatória.

Exemplo

```vue
<script lang="ts">
    export default {
        props: {
            categoria: {type: Object, required: true} // Aqui
        }
    }
</script>

<template>
    {{ categoria.nome }}
</template>
```

- Feito os passos acima, tu pode acessar todos os atributos do objeto categoria, caso tu queria acessar outro além do categoria.nome, basta abrir novamente as chaves e definir o outro atributo

Exemplo

```vue
<script lang="ts">
    export default {
        props: {
            categoria: {type: Object, required: true}
        }
    }
</script>

<template>
    {{ categoria.nome }}
    {{ categoria.imagem }} <!--Aqui-->
</template>
```

# Para aumentar a segurança do código tu pode usar o Proptype

- O Proptype além de habilitar o auto complete do vscode, vai te ajudar a não escrever o nome de uma props errado
- Para usar ele, tu vai precisar importar ele
- Dentro da propriedade que tu quer utilizar, da tipagem dela, tu vai definir q um as Proptype
- Depois disso tu vai abrir <> para definir um generic e utilizar a Interface q tu criou para a lista
- Dessa forma  tu vai ter certeza em não errar o nome de um atributo e vai ter mais segurança no código

Exemplo

```vue
<script lang="ts">
import type Icategoria from '@/interfaces/ICategoria';
import type { PropType } from 'vue';

    export default {
        props: {
            categoria: {type: Object as PropType<Icategoria>, required: true}
        }
    }
</script>

<template>
    {{ categoria.nome }}
</template>
```

