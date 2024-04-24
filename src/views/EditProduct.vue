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
    <div v-if="product" class="page">
		<h4 style="margin-top: 5rem; margin-left: 40%;">CHỈNH SỬA SẢN PHẨM</h4>
		<Productform
			  :product="product"
              @submit:product="updateProduct"
		/>
	</div>
</template>

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
              msg:"Sửa sản phẩm thành công",
              type:"success",
              duration:2000
              },
        product:null,
    }
  },
	components: {
        HeaderShop,
	    Productform,
        toastsVue
	},
	methods: {
    toastjs,
    async getproduct(id) {
			try {
				this.product = await ProductService.get(id);
			} catch (error) {
				console.log(error);
				this.$router.push({
					name: "notfound",
					params: { pathMatch: this.$route.path.split("/").slice(1) },
					query: this.$route.query,
					hash: this.$route.hash,
				});
			}
		},
        async updateProduct(data) {
            try {
                await ProductService.update(this.product._id,data);
                this.toastjs();
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
    created(){
        this.getproduct(this.$route.params.id);
    }
};
</script>
