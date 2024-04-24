<script>
import HeaderShop from '@/components/HeaderShop.vue';
import CartService from '../services/Cart.service';
import toastsVue from '../components/toasts.vue';
import toastsjs from '../assets/js/toasts.js'
import CartItem from "../components/CartItem.vue";
export default {
  data() {
    return {
      carts: [],
      toasts: {
        title: "",
        msg: "",
        type: "",
        duration: 0
      },
    }
  },
  components: {
    HeaderShop,
    toastsVue,
    CartItem
  },
  methods: {
    getiduser() {
      const user = JSON.parse(localStorage.getItem("user"));
      return user._id;
    },
    async getcarts() {
      try {
        this.carts = await CartService.get(this.getiduser());
      } catch (error) {
        console.log(error);
      }
    },
    async delcart(index) {
       this.toasts.title = "Deleted",
       this.toasts.msg = "XÓA SẢN PHẨM THÀNH CÔNG",
       this.toasts.type = "error",
       this.toasts.duration = 2000
       this.toastsjs();
      await CartService.delete(this.carts[index]._id)
      this.refeshlistcart();
    },
    toastsjs,
    refeshlistcart() {
      this.getcarts();
    },
    registerproduct() {
      if (this.carts.length > 0) {

        this.toasts.msg = "THANH TOÁN THÀNH CÔNG",
          this.toasts.type = "success",
          this.toasts.duration = 2000,
          this.toastsjs();
      } else {
        this.toasts.title = "Failed",
          this.toasts.msg = "Bạn chưa có sản phẩm",
          this.toasts.type = "error",
          this.toasts.duration = 2000,
          this.toastsjs();
      }
    },
    total() {
      var total = 0;
      for (var i in this.carts) {
        total += (this.carts[i].price * this.carts[i].quantity);
      }
      return total;
    }
  },
  created() {
    this.refeshlistcart();
  },
}
</script>

<template>
  <HeaderShop></HeaderShop>
  <toastsVue></toastsVue>
  <h2 style="text-align: center;">GIỎ HÀNG CỦA TÔI</h2>

  <section>
    <div class="container">
      <div class=" h-200">
        <div class="col-12">
            <div class="col-lg-8" style="margin-left: 15%;">
              <div>
                <hr class="my-4">
                <CartItem :refeshlistcart="refeshlistcart" :carts="carts" @deleted:cartIndex="delcart"></CartItem>
              </div>
            </div>
        </div>
      </div>
      <div class="container d-flex justify-content-center mt-5 mb-5">
        <div class="row g-3" style="margin-left: 20%;">
          <div>
            <h5 >TỔNG TIỀN: {{ total() }}.000VNĐ</h5>
            <div class="col-lg-4 text-align-center">
              <div class="d-flex justify-content-between mb-5 ">
                <button class="btn btn-dark" style="width: 30rem; margin-left: 3rem;">Thanh Toán</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
<style scoped>
@media (min-width: 1025px) {
  .h-custom {
    height: 100%;
  }
}

.card-registration .select-input.form-control[readonly]:not([disabled]) {
  font-size: 1rem;
  line-height: 2.15;
  padding-left: .75em;
  padding-right: .75em;
}

.card-registration .select-arrow {
  top: 13px;
}

.bg-grey {
  background-color: #ffffff;
}

@media (min-width: 992px) {
  .card-registration-2 .bg-grey {
    border-top-right-radius: 16px;
    border-bottom-right-radius: 16px;
  }
}

@media (max-width: 991px) {
  .card-registration-2 .bg-grey {
    border-bottom-left-radius: 16px;
    border-bottom-right-radius: 16px;
  }
}

.container {
  background-color: #FFF6EA;
}
</style>