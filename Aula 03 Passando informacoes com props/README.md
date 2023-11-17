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

# Reutilizando CSS

- Olhe os dois componentes abaixo
ConteudoPrincipal.vue
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

            <p class="paragrafo lista-vazia" v-else>
              <img src="../assets/images/icones/lista-vazia.svg" alt="Ícone de pesquisa">
              Sua lista está vazia, selecione ingredientes para iniciar.
            </p>
        </section>

        <SelecionarIngredientes />
    </main>
</template>
```

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
                {{ ingrediente }}
            </li>
        </ul>
    </article>
</template>
```

- Percebe, que os dois possuém uma ul, com praticamente a mesma estrutura?
- Então para reutilizar o css nesses dois componentes o que tu vai fazer é criar um novo Componente chamado Tag
- Esse componente vai receber um span, e  uma props para receber um texto, q nesse caasso vai ser é o ingrediente
- Ai dentro do componente Tag tu vai fazer a estilização para ambos, o resultado vai ser esse:

Resultado

ConteudoPrincipal.vue
```vue
<template>
    <main class="conteudo-principal">
        <section>
            <span class="subtitulo-lg sua-lista-texto">Sua lista:</span>
            <ul v-if="ingredientes.length" class="ingredientes-sua-lista">
                <li v-for="ingrediente in ingredientes" :key="ingrediente">
                    <Tag :texto="ingrediente"/> <!--Aqui-->
                </li>
            </ul>

            <p class="paragrafo lista-vazia" v-else>
              <img src="../assets/images/icones/lista-vazia.svg" alt="Ícone de pesquisa">
              Sua lista está vazia, selecione ingredientes para iniciar.
            </p>
        </section>

        <SelecionarIngredientes />
    </main>
</template>
```

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
                <Tag :texto="ingrediente" /> <!--Aqui-->
            </li>
        </ul>
    </article>
</template>
```

Tag.vue
```vue
<script lang="ts">
export default {
    props: {
        texto: { type: String, required: true }
    }
}
</script>

<template>
    <span class="tag">
        {{ texto }}
    </span>
</template>

<style scoped>
.tag {
    display: inline-block;
    border-radius: 0.5rem;
    min-width: 4.25rem;
    padding: 0.5rem;
    text-align: center;
    transition: 0.2s;
    color: var(--creme, #FFFAF3);
    background: var(--coral, #F0633C);
    font-weight: 700;
}
</style>
```

# Personalizando Estilos

- Nesta aula tu vai aprender como aplicar estilos com condicionais
- A forma para fazer isso no vue é:
- Criar uma nova classe de estilo dentro do componente, no ex vai ser .tag.ativa
Tag.vue
```vue
<style scoped>
.tag {
    display: inline-block;
    border-radius: 0.5rem;
    min-width: 4.25rem;
    padding: 0.5rem;
    text-align: center;
    transition: 0.2s;
    color: var(--cinza);
    background: var(--cinza-claro);
    font-weight: 400;
}

.tag.ativa { /* aqui */
    color: var(--creme, #FFFAF3);
    background: var(--coral, #F0633C);
    font-weight: 700;
}
</style>
```
- Para tu fazer a condicional, tu vai usar uma nova props no componente de Tag, essa prop vai ser um boolean
Tag.vue
```vue
<script lang="ts">
export default {
    props: {
        texto: { type: String, required: true },
        ativa: {type: Boolean, deefault: false} // Aqui
    }
}
</script>
```
### Observação

- O valor padrãp  de um Boolean para props já é false, então tu n precisa definir
- Como a sintaxe é mais curta, tu só vai especificar o tipo da prop, não precisa abrir chaves, tu pode resumir
Exemplo
```vue
<script lang="ts">
export default {
    props: {
        texto: { type: String, required: true },
        ativa: Boolean // Aqui
    }
}
</script>
```


## Agora que tu já criou a classe e definiu a props tu pode usar ela condicionalmente com as seguitnes opcões

### Opção 1

- Tu pode usar o v-bind, dps class e esspecificar um valor para ela
Exemplo
```vue
<script lang="ts">
export default {
    props: {
        texto: { type: String, required: true },
        ativa: {type: Boolean, deefault: false}
    }
}
</script>

<template>
    <span class="tag" :class="{ativa : true}">
        {{ texto }}
    </span>
</template>
```

### Opção 2

- Tu pode definir o valor, dela direto do componente pai. Assim, deixando ela mais dinâmica.
- Primeiro dentro do componente, tu vai definir para a condicionar, q ela vai receber esse valor da prop
Tag.vue
```vue
<script lang="ts">
export default {
    props: {
        texto: { type: String, required: true },
        ativa: {type: Boolean, deefault: false}
    }
}
</script>

<template>
    <span class="tag" :class="{ativa : ativa}">
        {{ texto }}
    </span>
</template>
```
- Depois dentro do Componente Pai, tu vai enviar a prop
ConteudoPrincipal.vue
```vue
<template>
  <main class="conteudo-principal">
    <section>
      <span class="subtitulo-lg sua-lista-texto">Sua lista:</span>
      <ul v-if="ingredientes.length" class="ingredientes-sua-lista">
        <li v-for="ingrediente in ingredientes" :key="ingrediente">
          <Tag :texto="ingrediente" :ativa="true"/> <!--Aqui-->
        </li>
      </ul>

      <p class="paragrafo lista-vazia" v-else>
        <img src="../assets/images/icones/lista-vazia.svg" alt="Ícone de pesquisa">
        Sua lista está vazia, selecione ingredientes para iniciar.
      </p>
    </section>

    <SelecionarIngredientes />
  </main>
</template>
```
- Uma observação interessante, quando você tem uma prop q é um boolean e você quiser q ela seja true, tu não precisa especificar o true, basta apenas chamar ela.
Ex
ConteudoPrincipal.vue
```vue
<template>
  <main class="conteudo-principal">
    <section>
      <span class="subtitulo-lg sua-lista-texto">Sua lista:</span>
      <ul v-if="ingredientes.length" class="ingredientes-sua-lista">
        <li v-for="ingrediente in ingredientes" :key="ingrediente">
          <Tag :texto="ingrediente" ativa/> <!--Aqui-->
        </li>
      </ul>

      <p class="paragrafo lista-vazia" v-else>
        <img src="../assets/images/icones/lista-vazia.svg" alt="Ícone de pesquisa">
        Sua lista está vazia, selecione ingredientes para iniciar.
      </p>
    </section>

    <SelecionarIngredientes />
  </main>
</template>
```

### Opção 3

- Há uma outra sintaxe que pode ser usada para o condicional de classes q é usando uma lista de classes
Tag.vue
```vue
<template> 
  <span :class="['tag', { ativa }]">
      {{ texto }}
    </span>
 </template>

```

-  Quando é um texto estático, ele será aplicado, e quando é um objeto, terá a mesma lógica de adicionar a classe se o valor booleano passado for true.
