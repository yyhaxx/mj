<template>
  <div class="receive">
    <back title="收款页面"></back>
    <div class="qr_code_box">
      <vue-qrcode :value="encodeAddress" :options="options"></vue-qrcode>
      <p><span v-text="address" id="foo"></span></p>
    </div>
    <div class="button_box">
      <button class="button first" @click="copyAddress" data-clipboard-target="#foo" id="copy-btn">复制地址</button>
      <button class="button" @click="logout">切换钱包</button>
    </div>
    <x-dialog :show = "show">
      <div class="address_list">
        <checklist :options="addressList" label-position="left" :max="1" @on-change="addressChange" :value="selectAddress"></checklist>
      </div>
    </x-dialog>
  </div>
</template>

<script>
  import Back from "../components/back";
  import VueQrcode from '@xkeshi/vue-qrcode'
  import { XDialog, Checklist} from "vux";
  export default {
    name: 'Receive',
    components: {
      Back,
      VueQrcode,
      XDialog,
      Checklist
    },
    data(){
      return {
        address: '',
        options: {
          size: 300,
          background: '#edf6f8',
          foreground: '#3c87b9'
        },
        show: false,
      }
    },
    computed: {
      encodeAddress(){
        return encodeURIComponent(this.address)
      },
      selectAddress(){
        let tempArr = []
        tempArr.push(this.address)
        return tempArr
      }
    },
    methods: {
      copyAddress(){
        this.$vux.toast.show({
          text: '复制成功'
        })
      },
      changeWallet(){
        this.show = true
      },
      addressChange(v, l){
        this.address = v[0];
        this.show = false
      },
      logout(){
        const _this = this 
        this.$vux.confirm.show({
          title:'切换钱包',
          content: '本操作将会清空你本地缓存，请妥善保存你的钱包地址和密码!',
          onConfirm () {
            localStorage.removeItem('address');
            localStorage.removeItem('password');
            _this.$router.push({
              path: '/start'
            })
          }
        })
      }
    },
    mounted(){
      this.address = localStorage.getItem('address')
      if(!this.address){
        this.$router.push({
          path: '/start'
        })
      }
      new Clipboard('#copy-btn')
    }
  }
</script>

<style scoped>
  .qr_code_box{
    margin: 0 auto;
    margin-top: 10px;
    width: 300px;
    background: #edf6f8;
    padding: 10px;
    font-size: 10px;  
  }
  .button_box{
    text-align: center;
    position: absolute;
    width: 100%;
    bottom: 20px;
  }
  .button{
    width: 220px;
    height: 50px;
    border: 1px solid rgba(200, 200, 200, 0.7);
    margin-top: 20px;
    border-radius: 6px;
    font-size: 16px;
    font-weight: 500;
    background: #eff2f3;
  }
  .first{
    margin-top: 120px;
    background: #36b9c8;
    color: #fff;
  }
  .address_list{
    width: 320px;
    overflow: scroll;
    max-height: 400px;
  }
</style>
<style>
  .receive .weui-cell__bd p{
    font-size: 8px!important;
  }
</style>


