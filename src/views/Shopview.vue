<script>
import SliderShop from '@/components/SliderShop.vue'
import HeaderShop from '@/components/HeaderShop.vue'
import ProductService from '../services/Product.service'
import { mapState } from "pinia";
import { useAuthStore } from "@/stores/Auth.store";
import toastsVue from '../components/toasts.vue';
export default {
    data() {
        return {
            Products: [],
        }
    },
    components: {
        HeaderShop,
        SliderShop,
        toastsVue
    },
    computed: {
        ...mapState(useAuthStore, {
            currentUser: "user",
        }),
    },
    methods: {
        async retrieveProduct() {
            try {
                this.Products = await ProductService.getAll();
            } catch (error) {
                console.log(error);
            }
        },
    },
    mounted() {
        this.retrieveProduct();
    },
}
</script>
<template>
    <div class="header" >
        <HeaderShop ></HeaderShop>
    </div>
    <toastsVue></toastsVue>
    <div class="slider">
        <SliderShop></SliderShop>
    </div>
    <div style="margin: 20px 100px;">
        <!-- <div class="d-sm-flex flex-wrap"> -->
        <div style="text-align: center; margin: 30px 0;" class="heading">
            <h3>SÁCH MỚI VỀ</h3>
        </div>
        <div class="d-sm-flex flex-wrap" id="giay" style="margin-left: 2rem;">
            <div class="card m-1" style="width: 19rem;" v-for="item in Products"
                v-show="item.categories === 'Sách'">
                <div>
                    <div>
                        <div class="image_item" v-for="img in item.img">
                            <img :src="img" class="card-img-top" alt="..." @click="nextdetailsproduct">

                        </div>
                    </div>
                </div>
         
                <div class="card-body product align-center ">
                    <h5 class="card-title text-center">{{ item.title }}</h5>
                    <h6 class="price text-center">{{ item.price }}.000VNĐ/Tuần</h6>
                    <router-link :to="{
                        name: 'details',
                        params: { id: item._id },
                    }">
                    <div class="d-grid ">
                        <button type="button" s class="btn btn-primary " @click="nextdetailsproduct">Xem chi tiết</button>
                    </div>
                        
                    </router-link>
                </div>
            </div>
        </div>
        <!-- </div> -->
    </div>
</template>
<style scoped>   .wrapper-img {
       width: 100%;
       height: 100%;
       overflow: hidden;
   }

   .image_slider {
       display: flex;
       transition: all .8s ease;

   }

   .image_slider:hover {
       transform: translateX(-100%);
   }

   .image_item {
       flex: 1 0 100%;
   }
</style>