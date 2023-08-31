<template>
    <Header />
    <div class="container m-auto my-4">
        <div class="manage-container w-[90%] m-auto border bg-white">
            <div class="manage mx-6">
                <h1 class="font-semibold text-4xl text-blue-600">Manage</h1>
            </div>

            <br class="underline-offset-8">
            <div class="form ">
                <form @submit.prevent="updateProfile()" action="">
                    <div class="w-full   px-3 rounded-lg">
                        <p class="px-1 font-semibold">Username</p>
                        <input class="w-full outline-blue-200 rounded-md border p-2" placeholder="Username" type="text"
                            name="" required v-model="username">
                    </div>

                    <div class="w-full my-3 px-3  rounded-lg">
                        <p class="px-1 font-semibold">email</p>
                        <input class="w-full outline-blue-200 rounded-md border p-2" placeholder="email" type="email"
                            name="" required v-model="email">
                    </div>
                    <div class="submit px-3 ">
                        <button type="submit" class=" bg-blue-700 text-white rounded-lg w-[100%] my-3 p-2">
                            Update
                        </button>
                    </div>

                </form>
                <div class="errors m-2" v-for="error in errors">
                    <p class=" text-center font-medium text-red-600">{{ error }}</p>
                </div>
            </div>
            <canvas id="myChart">
            </canvas>
        </div>


    </div>
</template>
  
<script setup>
import Chart from 'chart.js/auto';
import { onMounted } from 'vue';
import axios from 'axios';

const username = ref('')
const email = ref('')
const errors = ref([])
onBeforeMount(async () => {
    const token = sessionStorage.getItem("access")
    console.log(token)
    if (token) {
        axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
        const response = await axios.get('http://localhost:8000/user/profile/')
        username.value = response.data.username
        email.value = response.data.email
    }
    else {
        const router = useRouter()
        await router.push('/Account/Login/')
    }
})
onMounted(() => {
    const ctx = document.getElementById('myChart');

    new Chart(ctx, {
        type: 'bar',
        data: {
            labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
            datasets: [{
                label: '# of Votes',
                data: [12, 19, 3, 5, 2, 3],
                borderWidth: 1
            }]
        },
        options: {
            scales: {
                y: {
                    beginAtZero: true
                }
            }
        }
    });
})
const updateProfile = async () => {
    const formdata = new FormData()
    formdata.append('username', username.value)
    formdata.append('email', email.value)
    const token = sessionStorage.getItem("access")
    // console.log(token)
    if (token) {

        axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
        const response = await axios.put('http://localhost:8000/user/profile/', formdata)
        if (response.data === 'Username or Email already taken') {
            errors.value.push('Username or Email Already Taken')
        } else {
            const router = useRouter()
            await router.push('/Account/Manage/')
            console.log(response.data)

        }

    } else {
        const router = useRouter()

        await router.push('/Account/Login/')
    }

}

</script>