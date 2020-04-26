<!-- 审核及整改详情页 -->

<template>
    <view>
        <!-- 主背景图 -->
        <image class="main_background" src="../../static/background/back.png"></image>

        <!-- 如果是公司用户 -->
        <block v-if="userInfo.type === 'company'">
            <!-- 待整改隐患盒子 -->
            <view class="detail_box">
                <view class="detail_title">待整改隐患</view>
                <view class="image_list">
                    <block v-for="(item, index) in messageDetailList[0].imageList" :key="index">
                        <view class="item" :style="'background-image:url(' + item + ')'" ></view>
                    </block>
                </view>
                <text class="content">{{messageDetailList[0].content}}</text>
                <view class="level">隐患级别： II级</view>

                <view class="confirm_button" @click="showConfirmDialogClick">立即整改</view>

                <view class="detail_title">整改记录</view>
                <view class="history_list">
                    <block v-for="(item, index) in messageDetailList" :key="index">
                        <view v-if="index!==0" :class="'item ' + item.from">
                            <view class="image_list">
                                <block v-for="(sub_item, sub_index) in item.imageList" :key="sub_index">
                                    <view class="item"></view>
                                </block>
                            </view>
                            <text class="content">{{item.content}}</text>
                        </view>
                    </block>
                </view>
            </view>

            <!-- 确认提交对话框-->
            <view class="bottom_confirm_dialog" v-if="showConfirmDialog">
                <image class="close_button"  src="../../static/icon/close.png" @click="showConfirmDialogClick"></image>
                <view class="model_title">提交整改信息</view>
                <view class="model_title_placeholder"></view>

                <view class="bottom_confirm_dialog_content">
                    <view class="detail_title">概述整改信息</view>
                    <textarea class="reply_textarea" v-model="reviewContent" placeholder="请概述本次整改内容" ></textarea>

                    <view class="detail_title">上传图片</view>
                    <view :class="'upload_image_box' + (uploadImageList.length ===0?' no_item':'') ">
                        <block v-for="(item,index) in uploadImageList" :key="index">
                            <view class="item" @click="previewImage" :data-index="index"
                                  :style="'background-image: url(' + item + ')'">

                            </view>
                        </block>
                        <view class="item" v-if="uploadImageList.length < 9" @click="chooseImage">
                            <image class="icon" src="../../static/icon/camera.png"></image>
                            <view class="text">上传图片</view>
                        </view>
                    </view>

                    <view class="confirm_button" @click="uploadReviewInfo">立即提交</view>
                </view>

            </view>
            <view class="bottom_confirm_dialog_mask" v-if="showConfirmDialog" @click="showConfirmDialogClick"></view>
        </block>
        <!-- 公司用户内容结束 -->

        <!-- 如果是专家用户 -->
        <block v-if="userInfo.type === 'professor'">
            <!-- 待审核整改盒子 -->
            <view class="detail_box">
                <view class="detail_title">待审核整改</view>
                <view class="image_list">
                    <block v-for="(item, index) in messageDetailList[0].imageList" :key="index">
                        <view class="item" :style="'background-image:url(' + item + ')'" ></view>
                    </block>
                </view>
                <text class="content">{{messageDetailList[0].content}}</text>

                <view class="confirm_button" @click="showConfirmDialogClick">立即审核</view>

                <view class="detail_title">审核记录</view>
                <view class="history_list">
                    <block v-for="(item, index) in messageDetailList" :key="index">
                        <view v-if="index!==0" :class="'item ' + item.from">
                            <view class="image_list">
                                <block v-for="(sub_item, sub_index) in item.imageList" :key="sub_index">
                                    <view class="item"></view>
                                </block>
                            </view>
                            <text class="content">{{item.content}}</text>
                        </view>
                    </block>
                </view>
            </view>
            <!-- 待审核整改盒子 结束 -->

            <!-- 确认提交对话框-->
            <view class="bottom_confirm_dialog" v-if="showConfirmDialog">
                <image class="close_button"  src="../../static/icon/close.png" @click="showConfirmDialogClick"></image>
                <view class="model_title">提交审核信息</view>
                <view class="model_title_placeholder"></view>

                <view class="bottom_confirm_dialog_content">
                    <!-- 单选按钮用于通过审核或者不通过审核。不通过审核要填写审核信息 -->
                    <radio-group class="radio_box" @change="reviewConfirmChange">
                        <label class="item"><radio value="true" color="#29c07d" :checked="reviewConfirm === 'true'" />审核通过</label>
                        <label class="item"><radio value="false" color="#29c07d" :checked="reviewConfirm === 'false'" />审核驳回</label>
                    </radio-group>

                    <!-- 如果审核不通过要填写审核驳回的理由及附图 -->
                    <block v-if="reviewConfirm === 'false'">
                        <view class="detail_title">概述驳回理由</view>
                        <textarea class="reply_textarea" v-model="reviewContent" placeholder="请概述本次审核驳回的理由" ></textarea>

                        <!-- 级别 -->
                        <view class="detail_title">隐患级别</view>
                        <radio-group class="radio_box" @change="warningLevelChange">
                            <label class="item"><radio value="1" color="#29c07d" :checked="warningLevel === '1'" />I 级</label>
                            <label class="item"><radio value="2" color="#29c07d" :checked="warningLevel === '2'" />II 级</label>
                            <label class="item"><radio value="3" color="#29c07d" :checked="warningLevel === '3'" />III 级</label>
                        </radio-group>


                        <view class="detail_title">上传图片</view>
                        <view :class="'upload_image_box' + (uploadImageList.length ===0?' no_item':'') ">
                            <block v-for="(item,index) in uploadImageList" :key="index">
                                <view class="item" @click="previewImage" :data-index="index"
                                      :style="'background-image: url(' + item + ')'">

                                </view>
                            </block>
                            <view class="item" v-if="uploadImageList.length < 9" @click="chooseImage">
                                <image class="icon" src="../../static/icon/camera.png"></image>
                                <view class="text">上传图片</view>
                            </view>
                        </view>
                    </block>
                    <!-- 审核通过要签名 -->
                    <block v-else>
                        <view class="detail_title">请签名</view>
                        <view class="hand_writing" @click="doss" >
                            <block v-if="signImg === ''">
                                <view>点击签名</view>
                            </block>
                            <block v-else>
                                <image class="sign_image" :src="signImg" mode="widthFix"></image>
                            </block>
                        </view>
                    </block>

                    <view class="confirm_button" @click="uploadReviewInfo">立即提交</view>
                </view>

            </view>
            <view class="bottom_confirm_dialog_mask" v-if="showConfirmDialog" @click="showConfirmDialogClick"></view>
            <!-- 确认提交对话框 结束 -->
        </block>
        <!-- 专家用户内容结束 -->


        <!-- 绘图组件 -->
        <canvasSign canvasId="canvas1"  @close="close" @save="save" :visible="isShow" />

    </view>
</template>

<script>
    import canvasSign from "@/components/canvas_sign/canvas_sign";

    export default {
        data() {
            return {


                //用户信息。这里的用户信息应当通过登录时从服务器获取。数据只是测试用，具体字段根据接口返回而定。
                userInfo: {
                    user_name: '',
                    type: '' //company-公司用户  professor-专家用户
                },


                messageDetailList: [
                    {from:'professor', time: '2020-4-22 11:22:32', content: '1、车间部分电线裸露，未采用不燃套作保护；\n2、安全出口指示牌应急灯配置不足；\n3、未健全各项安全规章制度；\n4、未对作业人员进行安全生产的培训。',imageList: ['','']},
                    {from:'company', time: '2020-4-22 10:10:13', content: '1、裸露电线已用不燃套材料进行了保护；\n2、已重新采购指示牌和应急灯进行安装完毕；\n3、已建立健全本公司安全管理规章制度；\n4、目前已完成各岗位员工的安全生产的培训。', imageList: ['','']},
                    {from:'professor', time: '2020-4-22 08:30:50', content: '1、车间部分电线裸露，未采用不然套作保护；\n2、安全出口指示牌应急灯配置不足；\n3、未健全各项安全规章制度；\n4、未对作业人员进行安全生产的培训。', imageList: ['','']},
                    {from:'company', time: '2020-4-22 10:10:13', content: '1、裸露电线已用不燃套材料进行了保护；\n2、已重新采购指示牌和应急灯进行安装完毕；\n3、已建立健全本公司安全管理规章制度；\n4、目前已完成各岗位员工的安全生产的培训。', imageList: ['','']},
                    {from:'professor', time: '2020-4-22 08:30:50', content: '1、车间部分电线裸露，未采用不然套作保护；\n2、安全出口指示牌应急灯配置不足；\n3、未健全各项安全规章制度；\n4、未对作业人员进行安全生产的培训。', imageList: ['','']}
                ],

                showConfirmDialog:false,

                //公司用户用的
                reviewContent: "",
                uploadImageList: [],


                //专家用户用的
                reviewConfirm:'true',
                warningLevel: '1',

                //签名组件
                signImg:'',
                isShow:false,





            }
        },
        components: {
            canvasSign
        },
        methods: {

            //切换
            reviewConfirmChange: function(e){
                this.reviewConfirm = e.detail.value
            },
            warningLevelChange: function(e){
                this.warningLevel = e.detail.value
            },

            //展示确认对话框
            showConfirmDialogClick:function (e) {
                this.showConfirmDialog = !this.showConfirmDialog;
            },

            //预览图片
            previewImage:function (e) {
                let dataset = e.currentTarget.dataset;
                uni.previewImage({
                    current: parseInt(dataset.index),
                    urls: this.uploadImageList
                })
            },

            //选择图片
            chooseImage:function () {
                let _this = this;
                if(9-this.uploadImageList.length > 0){
                    uni.chooseImage({
                        count: 9-this.uploadImageList.length,
                        success:function (res) {
                            console.log(res);
                            for(let i=0;i<res.tempFilePaths.length;i++){
                                _this.uploadImageList.push(res.tempFilePaths[i]);
                            }
                        },
                        fail: function () {
                            uni.showToast({
                                title: '获取图片失败'
                            })
                        }

                    })
                }
                else{
                    uni.showToast({title: '您最多可选择9张图片'});
                }

            },

            //绘图组件
            doss(){
                this.isShow = true;
            },
            close(){
                this.isShow = false;
            },
            save(val){
                this.isShow = false;
                this.signImg = val
            },


            //提交整改内容
            uploadReviewInfo:function (e) {
                //循环调用接口上传图片，获得远程服务器地址，追加到新数组里

                //调用接口保存整改信息
            }

        },
        onLoad() {
            this.userInfo = uni.getStorageSync("user_info");
        }
    }
</script>

<style>

    .main_background{
        width: 100%;
        height: 570upx;
        display: block;
    }

    .detail_box{
        width: 600upx;
        min-height: 600upx;
        margin: -500upx auto 40upx auto;
        box-sizing: border-box;
        padding: 30upx;
        background-color: #ffffff;
        box-shadow: 0 0 20upx 0 rgba(0,0,0,0.1);
        position: relative;
        z-index: 2;
        border-radius: 30upx;
        overflow: hidden;
    }

    .detail_title{
        width: 100%;
        font-size: 36upx;
        color: #2ac080;
        margin: 20upx 0;
    }

    .detail_box>.image_list{
        width: 100%;
    }

    .detail_box>.image_list .item{
        width: 100%;
        height: 250upx;
        display: block;
        background-color: #f5f5f5;
        margin-bottom: 20upx;
        background-size: cover;
        background-position: center center;
        background-repeat: no-repeat;
        border-radius: 10upx;
    }

    .detail_box>.content{
        font-size: 20upx;
        color: #888888;
        background-color: #f5f5f5;
        display: block;
        box-sizing: border-box;
        padding: 30upx;
        border-radius: 10upx;
        line-height: 150%;
    }

    .detail_box>.level{
        width: fit-content;
        height: fit-content;
        padding: 10upx 40upx;
        font-size: 28upx;
        color: #ffffff;
        background-color: #2ac080;
        line-height: 100%;
        border-radius: 24upx;
        margin: 50upx auto 110upx auto;
    }

    .confirm_button{
        width: 400upx;
        height: 80upx;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 36upx;
        font-weight: bold;
        color: #ffffff;
        background-color: #29c07d;
        margin: 40upx auto;
        border-radius: 10upx;
        letter-spacing: 10upx;
    }

    .detail_box .confirm_button:active{
        opacity: .7;
    }

    .detail_box .history_list{
        width: 100%;
        display: flex;
        flex-flow: column;
    }

    .detail_box .history_list>.item{
        width: 90%;
        box-sizing: border-box;
        padding: 30upx;
        background-color: #f5f5f5;
        margin-bottom: 20upx;
        border-radius: 10upx;
        position: relative;
    }


    .detail_box .history_list>.item.company{
        margin: 0 0 20upx auto;
        background-color: #2ac080;
        color: #ffffff;
    }

    .detail_box .history_list>.item.company:after{
        content: '';
        display: block;
        position: absolute;
        z-index: 6;
        border-color: #2ac080 transparent;
        border-width: 0 10px 10px;
        right: -15px;
        top: 15px;
        border-style: solid;
        transform: rotate(90deg) scale(0.8,1);
    }

    .detail_box .history_list>.item.professor:after{
        content: '';
        display: block;
        position: absolute;
        z-index: 6;
        border-color: #f5f5f5 transparent;
        border-width: 0 10px 10px;
        left: -15px;
        top: 15px;
        border-style: solid;
        transform: rotate(-90deg) scale(0.8,1);
    }


    .detail_box .history_list>.item .image_list{
        width: 100%;
        display: flex;
        flex-wrap: warp;
        background-color: unset;
        padding: unset;
    }

    .detail_box .history_list>.item .image_list .item{
        width: 150upx;
        height: 150upx;
        background-color: rgba(0,0,0,0.5);
        border-radius: 10upx;
        margin: 0 10upx 10upx 0;
    }

    .detail_box .history_list>.item .content{
        font-size: 20upx;
        line-height: 150%;
        display: block;
    }





    /*确认对话框*/
    .bottom_confirm_dialog{
        width: 100%;
        position: fixed;
        z-index: 99;
        bottom: 0;
        background-color: #ffffff;
        animation: upShow 200ms ease-in-out;
        border-radius: 20upx 20upx 0 0;
        box-sizing: border-box;
        padding: 0 30upx;
    }

    .bottom_confirm_dialog_mask{
        position: fixed;
        z-index: 98;
        left: 0;
        right: 0;
        top: 0;
        bottom: 0;
        background-color: rgba(0,0,0,0.5);
        animation: hideShow 200ms ease-in-out;
    }

    .bottom_confirm_dialog .close_button{
        width: 50upx;
        height: 50upx;
        position: absolute;
        z-index: 2;
        right: 25upx;
        top: 25upx;
    }

    .bottom_confirm_dialog .model_title{
        width: 100%;
        height: 100upx;
        display: flex;
        align-items: center;
        justify-content: center;
        position: absolute;
        z-index: 0;
        top: 0;
        left: 0;
    }
    .model_title_placeholder{
        width: 100%;
        height: 100upx;
    }

    .bottom_confirm_dialog_content{
        width: 100%;
        min-height: 50vh;
        max-height: 70vh;
        overflow-y: scroll;
    }

    .bottom_confirm_dialog_content .reply_textarea{
        width: 100%;
        min-height: 100upx;
        max-height: 300upx;
        background-color: #f8f8f8;
        border-radius: 20upx;
        padding: 30upx;
        box-sizing: border-box;
        font-size: 25upx;
        color: #888888;
    }

    .upload_image_box{
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        justify-content: flex-start;
        /*padding: 30upx 0;*/
    }

    .upload_image_box.no_item{
        justify-content: center;
    }

    .upload_image_box .item{
        display: flex;
        flex-flow: column;
        align-items: center;
        justify-content: center;
        width: 200upx;
        height: 200upx;
        border: 1px solid #2ac080;
        background-size: cover;
        background-position: center center;
        background-repeat: no-repeat;
        margin: 5upx 13upx ;
    }

    .upload_image_box .item:active{
        opacity: .7;
    }

    .upload_image_box .item .icon{
        width: 60upx;
        height: 50upx;
        display: block;
    }

    .upload_image_box .item .text{
        font-size: 20upx;
        line-height: 100%;
        margin-top: 20upx;
        color: #2ac080;
    }

    .bottom_confirm_dialog_content .radio_box{
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 30upx auto;
    }

    .bottom_confirm_dialog_content .radio_box .item{
        color: #29c07d;
        transform:scale(0.7);
    }

    .bottom_confirm_dialog_content .hand_writing{
        width: 100%;
        min-height: 300upx;
        background-color: #f8f8f8;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .bottom_confirm_dialog_content .hand_writing .sign_image{
        width: 100%;
        display: block;
    }

</style>