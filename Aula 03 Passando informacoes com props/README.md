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

# Importando imagens dinamicamente

- Tu vai usar o vbind com template string, informando o caminho das imagens e depois a props para referenciar cada imagem

Exemplo

```vue
<template>
    <article class="categoria">
        <header class="categoria__cabecalho">
            <img :src="`/imagens/icones/categorias_ingredientes/${categoria.imagem}`" alt="" class="categoria__imagem">
        </header>
    </article>
</template>
```

### Vamos entender o que está acontecendo

- Tu definiu um v-for para o Componente SelecionarIngredientes, esté v-for terá 12 itens
- Tu passou  uma props para o componente CardCategoria, essa props é um objeto e dentro dela vai ter uma propriedade chamada imagem, que é uma string com o nome de uma imagem
- Dentro da pasta public tem uma pasta de imagens, com cada  imagem tendo o mesmo nome do atributo imagem q vem da props, q  tu está recebendo no componente
- Com o uso do v-bind e da template string, tu está definindo o caminho onde estão as imagens e depois o nome da imagem, q tu está recebendo como props
- Dessa forma tu está fazendo um caminho dinamico para cada imagem

