<script setup lang="ts">
import { useRoute, useRouter } from 'vue-router';
import { watch } from 'vue';

import useClient from '@/clients/composables/useClient';
import LoadingModal from '@/shared/components/LoadingModal.vue';

const route = useRoute();
const router = useRouter();
// const queryClient = useQueryClient();
//ponemos el + delante para convertirlo en number y que no sea un string
const { 
    client, 
    isLoading, 
    isError, 
    clientMutation,
    updateClient,
    isUpdating,
    isUpdatingSuccess,
 } = useClient( +route.params.id );

// const updateClient = async( client: Client ):Promise<Client> => {
//     // await new Promise( resolve => {
//     //     setTimeout( () => resolve(true), 2000 )
//     // });

//     const { data } = await clientsApi.patch<Client>(`/clients/${ client.id }`, client );
//     // es para tema de recarga de la página
//     // const queries = queryClient.getQueryCache().findAll(['clients?page='],{ exact: false});
//     // queries.forEach( query => query.fetch());

//     return data;
// }

// const clientMutation = useMutation( updateClient );
watch( clientMutation.isSuccess, () => {
        setTimeout(() => {
            clientMutation.reset()
        }, 2000)
    });

watch( isError, () => {
    if( isError.value )
    router.replace('/clients')
});

</script>

<template>


    <div v-if="client">
        <h3 v-if="isUpdating">Guardando...</h3>
        <h3 v-if="isUpdatingSuccess">Guardado</h3>

        <LoadingModal v-if="isLoading" />

        <div>
            <h1>{{ client.name }}</h1>
            <form @submit.prevent="updateClient(client!)" action="">
                <input 
                    type="text"
                    placeholder="Nombre"
                    v-model="client.name"
                />
                <br>
                <input 
                    type="text"
                    placeholder="Dirección"
                    v-model="client.address"
                />
                <br>
                <button 
                    type="submit"
                    :disabled="isUpdating"
                >Guardar</button>
            </form>
            <div>
                <code>
                    {{ client }}
                </code>
            </div>
        </div>
    </div>
</template>

<style scoped>
    input {
        width: 100%;
        padding: 5px 10px;
        margin-bottom: 10px;

    }
    button {
        margin-bottom: 10px;
    }
</style>