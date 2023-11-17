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