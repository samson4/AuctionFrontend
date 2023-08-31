<template>
    <Header />
    <div class="w-full text-center  my-3">
        <p>Already have an account? Then please <NuxtLink class="text-blue-600" to="/Account/Login/">sign in</NuxtLink>.</p>
    </div>

    <div class=" w-[40%] m-auto border  rounded-lg">
        <h1 class=" text-center bg-gray-200 h-[52px] py-1 "> <span class="font-bold text-3xl ">Create Your Account</span>
        </h1>
        <div class="m-7">
            <form @submit.prevent="registerUser()" class="signup" id="signup_form">
                <div class="justify-center mx-3">

                    <div class="w-full border my-3 rounded-lg">
                        <input class="w-full outline-blue-200 rounded-md border p-2" placeholder="Username" type="text"
                            name="" required v-model="username">
                    </div>

                    <div class="w-full border my-3 rounded-lg">
                        <input class="w-full outline-blue-200 rounded-md border p-2" placeholder="email" type="email"
                            name="" required v-model="email">
                    </div>


                    <div class="w-full border my-3 rounded-lg">
                        <input class="w-full outline-blue-200 rounded-md border p-2" placeholder="Password" type="password"
                            name="" required v-model="password">
                    </div>

                    <div class="w-full border my-3 rounded-lg">
                        <input class="w-full outline-blue-200 rounded-md border p-2" placeholder="Retype Password"
                            type="password" name="" required v-model="password2">
                    </div>
                    <button type="submit" class=" bg-blue-700 text-white rounded-lg w-[100%] my-3 p-2">
                        Sign Up
                    </button>
                </div>
            </form>
            <div class="errors m-2" v-for="error in errors">
                <p class=" text-center font-medium text-red-600">{{ error }}</p>
            </div>
        </div>
    </div>
</template>

<script setup>
import axios from 'axios';
const username = ref('')
const email = ref('')
const password = ref('')
const password2 = ref('')
const errors = ref([])

const registerUser = async () => {
    try {
        if (password.value !== password2.value) {
            errors.value.push('passwords do not match')
        }
        else if (password.value.length < 6) {
            errors.value.push('password is too short(must be greater than 5)')
        }
        else {
            const formdata = new FormData()
            formdata.append('username', username.value)
            formdata.append('email', email.value)
            formdata.append('password', password.value)

            const response = await axios.post('http://localhost:8000/user/register', formdata)
            const router = useRouter()
            router.push('/Account/Login/')
        }


    } catch (error) {
        if (error.response.data.username) {
            errors.value.push('A user with that username already exists.')
        }
        else {
            errors.value.push(error.response.data)
            console.log(errors.value[0].username)
        }
    }
}


</script>