<template class="g-gray-200">
    <Header />
    <div class=" w-full ">
        <div class="w-full text-center my-3">
            <p>If you have not created an account yet, then please
                <NuxtLink class="text-blue-600" to="/Account/Register/">sign up</NuxtLink> first.
            </p>
        </div>
        <div class="bg-white w-[40%]  m-auto border  my-4 rounded">
            <div class="w-full ">
                <h1 class="text-center bg-gray-200 h-12 text-2xl font-bold p-1">Welcome Back</h1>
                <div class="form m-7 ">
                    <form action="" @submit.prevent="loginUser()">

                        <div class="w-full my-4">
                            <input class="w-full outline-blue-200 rounded-md border p-2" placeholder="Username" type="text"
                                name="" id="" v-model="username">
                        </div>
                        <div class="w-full my-4">
                            <input class="w-full outline-blue-200 rounded-md border p-2" placeholder="Password"
                                type="password" name="" id="" v-model="password">
                        </div>
                        <div class="button">
                            <button class="primaryAction bg-blue-700 text-white rounded-lg w-[100%] p-2" type="submit">
                                Sign In
                            </button>
                        </div>
                    </form>
                    <div class="errors m-2" v-for="error in errors">
                        <p class=" text-center font-medium text-red-600">{{ error }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import axios from 'axios'
const username = ref('')
const password = ref('')
const errors = ref([])
const loginUser = async () => {

    console.log('submit')
    const loginData = new FormData()
    loginData.append('username', username.value)
    loginData.append('password', password.value)
    const url = 'http://localhost:8000/api/token/'
    try {
        const response = await axios.post(url, loginData)
        sessionStorage.setItem('access', response.data.access)
        sessionStorage.setItem('refresh', response.data.refresh)
        const router = useRouter()
        router.push('/')
    } catch (error) {

        errors.value.push(error.response.data.detail)

    }

}
</script>