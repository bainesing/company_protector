<template>
    <view>
        <!-- 主背景图 -->
        <image class="main_background" src="../../static/background/back.png"></image>

        <!-- 登录 -->
        <block v-if="displayMode === 'login'">
            <view class="login_box">
                <image class="main_title" src="../../static/background/title.png"></image>
                <input class="input_item" type="text" v-model="userName" placeholder="输入账号" />
                <input class="input_item" type="password" v-model="password" placeholder="输入密码" />
            </view>

            <view class="confirm_button" @click="bindLogin">立即登录</view>
            <view class="tips" @click="changeDisplayMode" data-mode="register">没有账号，点击注册</view>
        </block>
        <!-- 注册 -->
        <block v-else>
            <view class="login_box" style="margin-top: -440upx;">
                <input class="input_item" type="text" v-model="userName" placeholder="输入账号" />
                <input class="input_item" type="password" v-model="password" placeholder="输入密码" />
                <input class="input_item" type="number" v-model="register_phone" placeholder="注册电话" />
                <view class="modal_title">用户类型</view>
                <radio-group class="radio_box">
                    <label class="item"><radio value="company" color="#29c07d" checked="true" />企业用户</label>
                    <label class="item"><radio value="professor" color="#29c07d" />专家用户</label>
                </radio-group>
                <view class="modal_title">证书上传</view>
                <view class="certificate_box"></view>
            </view>

            <view class="confirm_button" @click="bindRegister">立即注册</view>
            <view class="tips" @click="changeDisplayMode" data-mode="login">已有帐号，点击登录</view>

        </block>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                //展示模式
                displayMode: 'login',

                //登录表单
                userName: '',
                password: '',

                //注册表单
                register_phone:'',
                register_type: 'company',
                certificate_url: ''
            }
        },
        onLoad() {

        },
        methods: {
            //登录
            bindLogin:function () {
                uni.switchTab({
                    url: '../index/index'
                })
            },

            //注册
            bindRegister:function(){

            },

            //点击注册或者点击登录
            changeDisplayMode:function (e) {
                this.displayMode = e.currentTarget.dataset.mode
            }
        }
    }
</script>

<style>

    .main_background{
        width: 100%;
        height: 570upx;
        display: block;
    }

    .login_box{
        width: 600upx;
        min-height: 400upx;
        margin: -300upx auto 0 auto;
        box-sizing: border-box;
        padding: 30upx 0;
        background-color: #ffffff;
        box-shadow: 0 0 20upx 0 rgba(0,0,0,0.1);
        position: relative;
        z-index: 2;
        border-radius: 30upx;
        overflow: hidden;
    }

    .login_box .main_title{
        width: 580upx;
        height: 353upx;
        display: block;
        margin: 30upx auto;
    }

    .login_box .input_item{
        width: 500upx;
        height: 80upx;
        margin: 40upx auto;
        box-sizing: border-box;
        padding: 0 20upx;
        line-height: 80upx;
        border: 1px solid #29c07d;
        font-size: 10pt;
        background-color: #f8f8f8;
    }

    .login_box .modal_title{
        font-size: 30upx;
        text-align: center;
        font-weight: bold;
        color: #29c07d;
        margin: 20upx 0;
    }

    .login_box .radio_box{
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .login_box .radio_box .item{
        color: #29c07d;
        /*font-size: 10pt;*/
        transform:scale(0.7);
    }

    .login_box .certificate_box{
        width: 500upx;
        height: 300upx;
        margin: 0 auto;
        border: 1px solid #29c07d;
        border-radius: 30upx;
    }

    .confirm_button{
        width: 500upx;
        height: 80upx;
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: #29c07d;
        color: #ffffff;
        border-radius: 20upx;
        margin: 80upx auto;
    }

    .confirm_button:active{
        opacity: .7;
    }

    .tips{
        width: fit-content;
        font-size: 10pt;
        color: #d4d4d4;
        text-decoration: underline;
        margin: 0 auto;
    }

</style>
