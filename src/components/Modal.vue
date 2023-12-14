<template>
    <Transition name="modal">
        <div class="modal" @click="closeModal">
            <div class="modal__block" @click.stop="">

                <h3 class="modal__block_title">{{ edit ? $t('changeNotes') : $t('AddNote') }}</h3>

                <div class="modal__block_inputs">
                    <label>
                        <span>Title</span>
                        <input v-model="title" type="text" placeholder="Введите название заметки">
                    </label>

                    <label>
                        <span>Content</span>
                        <textarea v-model="text" wrap="off" placeholder="Введите текст заметки"></textarea>
                    </label>
                </div>

                <div class="modal__block_btns">
                    <button class="modal__block_btns-btn del" @click="closeModal">{{$t('Cancel')}}
                    </button>
                    <button v-if="!edit" class="modal__block_btns-btn edit" @click="addNote">
                        {{$t('Add')}}</button>

                    <button v-else class="modal__block_btns-btn edit" @click="changeNote">
                        {{$t('Change')}}</button>
                </div>

            </div>
        </div>
    </Transition>
</template>

<script>

import { v4 as uuidv4 } from 'uuid';

export default {
    props: {
        edit: {
            typeof: Boolean
        },
        editedNote: {
            typeof: Object
        }
    },
    data() {
        return {
            title: '',
            text: '',
        }
    },
    methods: {
        closeModal() {
            this.$emit('close')
            this.title = this.text = ''
        },
        addNote() {
            if (this.title != '' && this.text != '') {
                const note = {
                    id: uuidv4(),
                    title: this.title,
                    text: this.text,
                    // 19.04.2023
                    date: new Date().toLocaleDateString()
                }
                this.$emit('addNote', note)
                this.closeModal()
            }

        },
        changeNote() {
            let newEditNote = {
                id: this.editedNote.id,
                title: this.title,
                text: this.text,
                date: new Date().toLocaleDateString()
            }
            this.$emit('changedNote', newEditNote)
            this.closeModal()
        }
    }
}
</script>


