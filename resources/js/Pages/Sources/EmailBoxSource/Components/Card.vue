<script setup>

import {Link, useForm, usePage} from "@inertiajs/vue3";
import {useToast} from "vue-toastification";
import Settings from "@/Pages/Sources/Cards/Settings.vue";
import Clipboard from "@/Components/Clipboard.vue";
import {computed} from "vue";
const toast = useToast();

const props = defineProps({
    source: Object
})

const form = useForm({})

const email = computed(() => {
    return `${usePage().props.email_name}+${ props.source.slug }@laralamma.ai`
})

const run = (source) => {
    form.post(route('collections.sources.run', {
        source: source.id
    }), {
        onStart: params => {
          toast("Running");
        },
        preserveScroll: true,
        onSuccess: () => {
            toast.success('Source is running');
        }
    });
}

</script>

<template>
    <div class="card rounded-none w-96  shadow-xl border border-neutral" :key="source.id">
        <div class="card-body">
            <Settings :source="source"/>
            <div class="card-actions justify-end">
                <button @click="run(source)" type="button" class="btn btn-primary rounded-none">Run</button>
                <Link :href="route('collections.sources.email_box_source.edit', {
                                    collection: source.collection_id,
                                    source: source.id
                                })" class="btn btn-primary rounded-none">Edit</Link>
            </div>
        </div>
    </div>
</template>

<style scoped>

</style>
