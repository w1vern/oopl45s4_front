<script setup>

import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const showModal = ref(false)
const matchId = ref('')
const showError = ref(false)
const errorMessage = ref('Incorrect ID')

async function createMatch() {
    const res = await apiLobbiesCreate()
    if (res.status) {
        router.push('match/' + res.ID)
    }
}


async function connectToMatch() {
    showModal.value = true
}

async function closeModal() {
    showModal.value = false
}

async function checkID() {
    if (await apiLobbiesConnect(matchId.value) == 200) {
        showModal.value = false
        router.push('/match/' + matchId.value)
    }
    else {
        showError.value = true
    }
}


</script>

<template>
    <div class="lobby_page">
        <div class="lobby_form">
            <div class="top_part">
                <button_default :value="'create match'" @click="createMatch" type="button" color="#550066"
                    hover_color="#770088" font_color="white" size="15px" />
                <button_default :value="'connect to match'" @click="connectToMatch" type="submit" color="#550066"
                    hover_color="#770088" font_color="white" size="15px" />
            </div>
            <div class="main_part">
                <div class="main_part_header">

                </div>
                <div class="main_part_content">

                </div>
            </div>
        </div>
        <div class="modal" v-if="showModal">
            <div class="modal_content">
                <span class="close_button" @click="closeModal">&times;</span>
                <input id="test_field_1" type="text" v-model="matchId" placeholder="Enter match ID" />
                <button_default :value="'connect'" @click="checkID" type="button" color="#111010" hover_color="#222020"
                    font_color="white" size="15px" />
                <p class="error_message" v-if="showError">{{ errorMessage }} </p>
            </div>
        </div>
    </div>
</template>

<style scoped>
.lobby_page {
    justify-content: center;
    align-content: center;
}


.lobby_form {
    display: flex;
    flex-direction: column;
    margin-top: 10%;
    margin-left: 20%;
    margin-right: 20%;
}

.top_part {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    width: 100%;
    height: max-content;
}

.connect_block {
    border: 1px solid black;
    width: max-content;
    height: max-content;
    padding: 15px;
}

.button:hover {
    background-color: #770088;
}

.button {
    background-color: #550066;
    color: white;
    border: none;
    padding: 5px;
}

.main_part {
    display: flex;
    flex-direction: column;
}

.main_part_header {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}

.modal_content {
    position: absolute;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    justify-content: center;
    align-content: center;
    align-items: center;
    display: flex;
    flex-direction: row;
    gap: 3px;
    background-color: rgba(0, 0, 0, 0.5);
}

.modal_content>* {
    background-color: #111010;
    color: white;
    width: max-content;
    height: max-content;
    border: none;
    border-radius: 2px;
    padding: 5px;
    caret-color: transparent;
}

.modal_content>*:focus {
    outline: 2px solid #550066;
    ;
}

.close_button {
    background-color: #111010;
    user-select: none;
    border-radius: 4px;
}

.close_button:hover {
    background-color: #222020;
}
</style>