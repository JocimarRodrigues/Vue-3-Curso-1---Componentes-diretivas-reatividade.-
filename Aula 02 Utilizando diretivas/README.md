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