<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router'

const incorrect_data = ref(false)

const form = ref({login:ref(''),password1:ref(''),password2:ref('')})

const error = ref('This login is already in use or the passwords do not match')

const router = useRouter()


async function SignUp()
{
    const data = await apiUsersRegister(form)
    if(data == 200)
    {
        router.push('/login')
    }
    else {
        incorrect_data.value = true
    }
}

</script>

<template>
    <div class="register_page">
        <div class="register_form">
            <div class="description">
                <p>Registration</p>
            </div>
            <div class="form">
                <form class="form" @submit.prevent="SignUp">
                    <p>
                        <input class="field_input" id="login" name="login" placeholder="Login" required="" type="text" v-model="form.login">
                    </p>
                    <p>
                        <input class="field_input" id="password1" name="password" placeholder="Password" required=""
                            type="password" v-model="form.password1">
                    </p>
                    <p>
                        <input class="field_input" id="password2" name="password" placeholder="Repeat Password"
                            required="" type="password" v-model="form.password2">
                    </p>
                    <p>
                        <button_default :value="'Sign Up'" type="submit" color="#550066" hover_color="#770088"
                            font_color="white" size="15px" />
                    </p>
                    <p v-if="incorrect_data" class="auth_error">
                        {{ error }}
                    </p>
                </form>
            </div>
        </div>
        <a href="/login" class="login">Sign In</a>
    </div>
</template>

<style scoped>
.register_page {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-content: center;
    align-items: center;
    height: 100%;
}

.register_page * {
    caret-color: transparent;
    border: none;
}

.register_page * ::selection{
   background-color: #550066;
}

.register_form {
    display: flex;
    justify-content: center;
    align-content: center;
    flex-direction: column;
    border-radius: 10px;
    padding: 20px;
    text-align: center;
    font-size: 20px;
    background-color: #111010;
    gap: 16px;
}

.description {
    font-size: 28px;
    user-select: none;
}

.field_input {
    box-sizing: border-box;
    width: 180px;
    height: 30px;
    border-radius: 3px;
    background-color: #222020;
    color: #ffffff;
    padding: 2px 8px 2px 8px;
}

.field_input:focus {
    caret-color: #9900aa;
    outline: none;
    outline: 2px solid #550066;
}

.form {
    background-color: #111010;
    display: flex;
    flex-direction: column;
    gap: 16px;
    justify-content: center;
}

.remember_me_container {
    width: max-content;
    font-size: 18px;
}

.auth_error {
    color: red;
    font-size: 15px;
    width: 100%;
}


.login {
    user-select: none;
    font-size: 20px;
    padding: 10px;
    color: #880099;
    text-decoration: none;
    font-weight: 900;
}

.login:hover {
    color: #aa00bb;
}


</style>
