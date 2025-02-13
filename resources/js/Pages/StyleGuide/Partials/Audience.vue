<script setup>
import { ref } from 'vue';
import { Link, router, useForm } from '@inertiajs/vue3';
import ActionMessage from '@/Components/ActionMessage.vue';
import FormSection from '@/Components/FormSection.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import SecondaryButton from '@/Components/SecondaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import TextArea from "@/Components/TextArea.vue";
import AudienceEdit from "@/Pages/StyleGuide/Partials/AudienceEdit.vue";

const props = defineProps({
    audiences: Object,
});

const audience = ref({})

const show_add_form = ref(false);

const show_edit_form = ref(false);
const showAddForm = () => {
    show_add_form.value = !show_add_form.value;
}
const showEditForm = (styleItem) => {
    console.log('showEditForm', styleItem);
    show_edit_form.value = false;
    audience.value = {};
    audience.value = styleItem;
    show_edit_form.value = true;
}

const hideEditForm = () => {
    show_edit_form.value = false;
    audience.value = {};
}

const createForm = useForm({
    _method: 'POST',
    name: "",
    content: "",
});

const addStyle = () => {
    createForm.post(route('style_guide.create.audience'), {
        errorBag: 'updateAudienceInformation',
        preserveScroll: true,
        onSuccess: () => {
            createForm.reset();
            show_add_form.value = false;
        }
    },
    );
}



</script>

<template>
    <FormSection @submitted="addStyle">
        <template #title>
            Audience
        </template>

        <template #description>
            Here you can create an audience that the LLM will output the response for.
        </template>
        <template #intro>
            <h2>Existing Audiences </h2>
            <div v-auto-animate>
                <div class="text-sm text-secondary"  v-if="audiences.length === 0">
                    No Audiences Yet! Start adding below.
                </div>
                <div>
                    <PrimaryButton @click="showAddForm" v-if="!show_add_form">
                        Add Audience
                    </PrimaryButton>
                </div>
                <div class="mt-10 w-full border border-secondary rounded-lg p-10" v-auto-animate>
                    <div
                        v-if="audiences.length > 0 && show_edit_form === false"
                        class="flex flex-wrap gap-4 mt-2 w-full justify-center items-center">
                        <div v-for="styleItem in audiences" :key="styleItem.id">
                            <div class="flex
                            w-full items-center gap-2
                            bg-pink-600 text-white
                            justify-start border-secondary border text-sm py-2 px-2">
                                {{ styleItem.name }}
                                <button type="button"
                                        :class="{
                                            'opacity-25': styleItem.id !== styleItem.id
                                        }"
                                        @click="showEditForm(styleItem)"
                                        class="btn btn-sm btn-outline rounded-none  text-white">
                                    edit
                                </button>
                            </div>
                        </div>
                    </div>
                    <div v-if="show_edit_form">
                        <AudienceEdit :audience="audience">
                            <button
                                class="btn btn-outline rounded-none"
                                @click="hideEditForm()">
                                Close
                            </button>
                        </AudienceEdit>
                    </div>
                </div>
            </div>
        </template>

        <template #form v-if="show_add_form">
            <div class="col-span-6 sm:col-span-4">
                <InputLabel for="name" value="Title" />
                <input
                    id="name"
                    v-model="createForm.name"
                    type="text"
                    class="mt-1 block input input-bordered w-full "
                    placeholder="In the voice of Mark Twain"
                />
                <InputError :message="createForm.errors.name" class="mt-2" />
            </div>

            <div class="col-span-6 sm:col-span-4">
                <InputLabel for="data" value="Content" />
                <textarea
                    rows="15"
                    id="data"
                    v-model="createForm.content"
                    class="textarea textarea-bordered w-full"
                    placeholder="The audience is non-technical but have an interest for new gadgets
"
                />
                <InputError :message="createForm.errors.data" class="mt-2" />
            </div>

        </template>

        <template #actions>

            <ActionMessage :on="createForm.recentlySuccessful" class="me-3">
                Saved.
            </ActionMessage>

            <PrimaryButton
                v-if="show_add_form"
                :class="{ 'opacity-25': createForm.processing }" :disabled="createForm.processing">
                Add
            </PrimaryButton>
        </template>
    </FormSection>
</template>
