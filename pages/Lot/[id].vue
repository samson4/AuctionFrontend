<template>
    <Header />
    <div class="bg-gray-100 ">
        <div class="w-[60%] bg-white m-auto  p-2">
            <div class="upper-container">
                <button class="border rounded ">Return to Auction</button>
                <div class="flex justify-between ">
                    <div class="w-[500px]">
                        <h1 class="text-blue-800 text-2xl">{{ data.title }}</h1>
                    </div>
                    <div class="">
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
                </div>
            </div>
            <div class="content border flex justify-evenly  border-y-2">
                <div class="flex left-container w-[40%] h-[100%] m-4">
                    <div class="image-container">
                        <img src="https://online-auction.state.gov/Image/Get?id=848045f9-1d99-4c9f-931d-7df95bdeb43a&size=2"
                            alt="">
                    </div>
                </div>
                <div class="right-container px-3">
                    <div class="bg-gray-100 rounded w-16 my-3 h-6 mx-1 border">
                        <p class="rounded-md">LOT 1</p>
                    </div>
                    <div class="bids">
                        <div class="flex gap-8 ">
                            <div class="bids ">
                                <p>Bids</p>
                                <div class="flex justify-around">
                                    <div class="icon border bg-gray-200 w-8 ">
                                        <IconsPointing class="m-1" />
                                    </div>
                                    <div class="price border w-20 text-right px-2">
                                        <p>0</p>
                                    </div>
                                </div>
                            </div>
                            <div class="current-price">
                                <p>Current Price</p>
                                <div class="flex ">
                                    <div class="icon border bg-gray-200 w-8 ">
                                        <IconsPointing class="m-1" />
                                    </div>
                                    <div class="price border w-20 text-right px-2">
                                        <p>{{ data.Price }}</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="bid-info ">
                        <div class="flex text-blue-700 text-xl my-3 gap-1 border-b-2 border-blue-700 w-full">
                            <IconsInfo class="my-1" />
                            <h1 class="">
                                BID INFORMATION
                            </h1>
                        </div>
                        <div class="bid form">
                            <div class="increase ">
                                <div class="bids">
                                    <div class="flex justify-center">
                                        <div class="bids ">
                                            <p class="w-[180px]">Minimum increase</p>
                                            <div class="flex">
                                                <div class="icon border bg-gray-200 w-8 ">
                                                    <IconsDollar class="m-1" />
                                                </div>
                                                <div class="price border w-20 text-right px-2 py-1 ">
                                                    <p>{{ data.Price }}</p>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="current-price">
                                            <p class="w-[180px]">Maximum Increase</p>
                                            <div class="flex ">
                                                <div class="icon border bg-gray-200 w-8 ">
                                                    <IconsDollar class="m-1" />
                                                </div>
                                                <div class="price border w-20 text-right px-2 py-1 ">
                                                    <p>{{ data.Price }}</p>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="increase ">
                                <div class="bids">
                                    <div class="flex ">
                                        <div class="bids ">
                                            <p class="w-[180px]">My Last bid</p>
                                            <div class="flex">
                                                <div class="icon border bg-gray-200 w-8 ">
                                                    <IconsDollar class="m-1" />
                                                </div>
                                                <div class="price border w-20 text-right px-2 py-1 ">
                                                    <p>0</p>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="current-price">
                                            <p class="w-[180px]">Maximum Increase</p>
                                            <form action="" @submit.prevent="submitOffer()">
                                                <div class="flex ">
                                                    <div class="icon border bg-gray-200 w-8 ">
                                                        <IconsDollar class="m-1" />
                                                    </div>
                                                    <div class="price border w-20 ">
                                                        <input
                                                            class="w-20 outline-blue-300 px-2 py-1 text-right font-medium  text-red-700"
                                                            type="text" :value="data.Price">
                                                    </div>
                                                </div>
                                                <div
                                                    class="submit-btn  h-8 my-3 w-28  text-center  text-white bg-blue-400  rounded-sm">
                                                    <button type="submit" class="">Submit</button>
                                                </div>
                                            </form>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

        </div>



    </div>
</template>

<script setup>
const { id } = useRoute().params
const { data } = await useFetch(`http://localhost:8000/Auction/lots/${id}/`, { key: id })
console.log(data)

</script>
