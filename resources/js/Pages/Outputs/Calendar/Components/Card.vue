<script setup>

import {Link} from "@inertiajs/vue3";
import Settings from "@/Pages/Outputs/Components/Settings.vue";

const props = defineProps({
    output: Object
})
</script>

<template>
    <div class="card rounded-none w-96  shadow-xl  border border-neutral">
    <div class="card-body">
            <Settings :output="output"/>

            <div class="card-actions justify-between flex items-center">
                <span class="badge badge-default">{{ output.type_formatted}}</span>
                <div class="flex justify-end gap-2 items-center">
                    <Link class="link" :href="route('calendar.show', {
                        collection: output.collection_id,
                    })">visit</Link>
                    <Link :href="route('collections.outputs.calendar_output.edit', {
                                        collection: output.collection_id,
                                        output: output.id
                                    })" class="btn btn-primary rounded-none">Edit</Link>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>

</style>
