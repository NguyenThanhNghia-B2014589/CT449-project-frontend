
<script>
import toastjs from "../assets/js/toasts";
import toastsVue from "../components/toasts.vue";
import ProductService from "../services/Product.service";
import HeaderShop from "../components/HeaderShop.vue";
import Productform from "../components/Productform.vue";
export default {
  data(){
    return {
         toasts:{
              title:"Success",
              msg:"Thêm sản phẩm thành công",
              type:"success",
              duration:2000
              },
    }
  },
  components: {
    HeaderShop,
    Productform,
    toastsVue
    },
	methods: {
    toastjs,
        async addproduct(data) {
            try {
                await ProductService.create(data);
                this.toastjs();
                setTimeout(()=>{
                  location.reload();
                },2000);
            }catch(error) {
                console.log(error);
                    this.toasts.title = "Warning",
                    this.toasts.msg="Tài khoản không phải ADMIN",
                    this.toasts.type = "warn",
                    this.toasts.duration=2000
                    this.toastjs();
                }
            },  
	},
};
</script>
<template>
<nav class="navbar fixed-top navbar-expand-lg navbar-dark bg-dark">
        <a class="navbar-brand" href=""><router-link to="/admin" class="text-white">
                            TRANG CHỦ</router-link></a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarTogglerDemo01"
            aria-controls="navbarTogglerDemo01" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarTogglerDemo01">
            <ul class="navbar-nav ms-auto">
                <li class="nav-item">
                    <a class="nav-link" href=""><router-link to="/ListKH" class="text-white">Danh Sách Người
                            Dùng</router-link></a>
                </li>

                <li class="nav-item">
                    <a class="nav-link" href=""><router-link to="/ListSP" class="text-white">Danh Sách Sản
                            Phẩm</router-link></a>
                </li>
                <!-- <li class="nav-item">
            <a class="nav-link" href="">Download</a>
        </li> -->
            </ul>
        </div>
    </nav>
	<div class="page">
		<div>
			<h4 style="margin-top: 7rem; margin-left: 45%;">THÊM SẢN PHẨM</h4>
			<Productform
				:product="{img:[]}"
        @submit:product="addproduct"
        :resetAfterSubmit="false"
			/>
		</div>
	</div>
</template>
