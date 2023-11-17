# Outra forma de tu exportar um componente é dessa forma:

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