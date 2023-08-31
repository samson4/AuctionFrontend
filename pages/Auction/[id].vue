<template>
    <Header />
    <div class=" bg-gray-100">
        <div class="container flex max-w-[70%]  m-auto my-6 bg-white" v-for="lot in data">
            <div class="left-container w-[40%] text-center p-3">
                <div class="img-div ">
                    <div class="bg-white h-full overflow-hidden">
                        <img src="https://online-auction.state.gov/Image/Get?id=b6fa7a64-92e8-4d1e-806b-58670bcc6383&size=2"
                            class="object-cover hover:scale-110 delay-150 transition-all" alt="">
                    </div>

                </div>
                <div class="bid-status border border-blue-400 rounded-md w-full my-2 p-[6px]">
                    <h1 class="text-blue-400 text-sm">You did not bid this Lot</h1>
                </div>
                <div class="view-lot bg-[#3ba9e8] h-12 py-3 rounded-md ">
                    <NuxtLink :to="`/lot/${lot.id}/`" class="text-white text-base font-semibold">VIEW THIS LOT</NuxtLink>
                </div>
            </div>
            <div class="right-container w-full p-2">
                <div class="flex justify-between ">
                    <div>
                        <h1 class="font-semibold text-3xl"> {{ lot.title }}</h1>
                    </div>
                    <div class="status text-center text-white text-sm">
                        <div class="flex w-32 h-7 m-2 bg-[#73a839] rounded-md justify-center gap-2">
                            <div class="">
                                <IconsHammer class="w-5 h-7" />
                            </div>

                            <div class="my-1">

                                <p>Active</p>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="flex justify-between my-3">
                    <div class="">
                        <p class="font-bold text-sm ">Current price</p>
                        <p class=" border rounded-md">fr.<span class="">{{ lot.Price }}</span></p>
                    </div>
                    <div class="">
                        <p class="font-bold text-sm ">Condition</p>
                        <input type="text" :value="lot.condition" disabled class="border rounded-md">

                    </div>
                </div>
                <p class="font-bold text-sm">Description</p>
                <div class="border rounded-sm p-2">
                    <p>{{ lot.description }}</p>
                </div>

            </div>
        </div>

    </div>
    <div class="subscribe bg-[#dd5600] w-[70%] h-12 m-auto  flex justify-center gap-3 text-white rounded-md">
        <div>
            <IconsPen class="my-4" />
        </div>
        <button @click="subscribetoAuction">
            Subscribe to place bids
        </button>
    </div>
    <AgreementModal v-show="showAgreementModal" />
</template>

<script setup>
import axios from 'axios';

const showAgreementModal = ref(false)
const { id } = useRoute().params
const { data } = await useFetch(`http://localhost:8000/Auction/${id}/lots/`, { key: id })
console.log(data.value)
const subscribetoAuction = async () => {
    // showAgreementModal.value = !showAgreementModal.value

    const token = sessionStorage.getItem("access")
    console.log(token)
    if (token) {
        axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
        // const config = {
        //     headers: {
        //         Authorization: `Bearer ${token}`
        //     }
        // }
        const url = `http://localhost:8000/Auction/subscribe/${id}/`
        const response = await axios.post(url)
        const router = useRouter()
        await router.push('/')
    }


}
</script>

<style scoped></style>