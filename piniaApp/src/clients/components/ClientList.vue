<script setup lang="ts">import { RouterLink } from 'vue-router';
import type { Client } from '@/clients/interfaces/client';


interface Props {
    clients: Client[]
}

const props = defineProps<Props>()


</script>

<template>
    <div>
        <ul>
            <li 
                v-for="client of props.clients" 
                :key="client.id">
                <RouterLink :to="{
                    name: 'client-id',
                    params: { id: client.id },
                }">
                    {{ client.name }}
                </RouterLink>
            </li>
        </ul>
    </div>
</template>

<style scoped>

</style>