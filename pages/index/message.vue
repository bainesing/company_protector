<template>
    <view>
        <view class="top_box">
            <image class="message_main_background" src="../../static/background/message_back.jpg"></image>
            <view class="top_box_content">
                <block v-if="userInfo.type!==''">
                    <view class="hello">
                        <text>您是{{userInfo.type==='company'?'企业':'专家'}}用户</text>
                        <text style="margin: 0 20upx;">|</text>
                        <text @click="logout">退出登录</text>
                    </view>
                </block>
                <block v-else>
                    <view class="hello">
                        <text>您尚未登录</text>
                        <text style="margin: 0 20upx;">|</text>
                        <text @click="logout">点击登录</text>
                    </view>
                </block>

                <view class="order_by_box">
                    <view class="title">排序方式</view>
                    <view class="order_by_list">
                        <block v-for="(item, index) in orderByList" :key="index">
                            <view :class="'item' + (currentOrderByItem === index?' active':'')"
                                    @click="changeOrderByItem" :data-index="index">{{item.name}}</view>
                        </block>
                    </view>
                </view>
            </view>
        </view>

        <!-- 如果是专家用户 -->
        <block v-if="userInfo.type === 'professor'">

            <block v-if="professorMessageList.length >0">
                <view class="message_list">
                    <block v-for="(item, index) in professorMessageList" :key="index">
                        <view class="item">
                            <!-- 缩略图用背景展示 -->
                            <view class="picture" style="background-image: linear-gradient(to bottom, #888888,#888888)">
                                <block v-if="item.status === 0">
                                    <view class="cover danger">
                                        <image class="icon" src="../../static/icon/danger_icon.png"></image>
                                    </view>
                                </block>
                                <block v-else>
                                    <view class="cover success">
                                        <image class="icon" src="../../static/icon/success_icon.png"></image>
                                    </view>
                                </block>
                            </view>
                            <view class="title">{{item.title}}</view>
                            <view v-if="item.status === 0" class="button" @click="viewMessageDetail" :data-index="index">查看详情</view>
                            <view v-else class="button" @click="viewMessageDetail" :data-index="index">提交审核</view>
                        </view>
                    </block>
                </view>
            </block>
            <block v-else>
                <view class="no_item_box">
                    <image class="icon" src="../../static/icon/no_item.png"></image>
                    <view class="text">没有消息</view>
                </view>
            </block>

        </block>
        <block v-else-if="userInfo.type === 'company'">

            <block v-if="messageList.length >0">
                <view class="message_list">
                    <block v-for="(item, index) in messageList" :key="index">
                        <view class="item">
                            <!-- 缩略图用背景展示 -->
                            <view class="picture" style="background-image: linear-gradient(to bottom, #888888,#888888)">
                                <block v-if="item.status === 0">
                                    <view class="cover danger">
                                        <image class="icon" src="../../static/icon/danger_icon.png"></image>
                                    </view>
                                </block>
                                <block v-else>
                                    <view class="cover success">
                                        <image class="icon" src="../../static/icon/success_icon.png"></image>
                                    </view>
                                </block>
                            </view>
                            <view class="title">{{item.title}}</view>
                            <view v-if="item.status === 0" class="button" @click="viewMessageDetail" :data-index="index">提交整改</view>
                            <view v-else class="button" @click="viewMessageDetail" :data-index="index">查看详情</view>
                        </view>
                    </block>
                </view>
            </block>
            <block v-else>
                <view class="no_item_box">
                    <image class="icon" src="../../static/icon/no_item.png"></image>
                    <view class="text">没有消息</view>
                </view>
            </block>


        </block>

        <block v-else>
            <view class="no_item_box">
                <image class="icon" src="../../static/icon/no_item.png"></image>
                <view class="text">没有消息</view>
            </view>
        </block>


    </view>
</template>

<script>
    export default {
        data() {
            return {
                //排序方式列表。请求消息接口需要传排序方式参数，这个参数要从这里取。
                orderByList: [
                    {name: '时间',en_name: 'time'},
                    {name: '级别', en_name: 'level'},
                    {name: '状态', en_name: 'status'}
                ],
                currentOrderByItem: 0,

                //用户信息。这里的用户信息应当通过登录时从服务器获取。数据只是测试用，具体字段根据接口返回而定。
                userInfo: {
                    user_name: '',
                    type: '' //company-公司用户  professor-专家用户
                },

                //整改列表。如果是专家用户，这里存储待审核列表
                messageList: [
                    {title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
                    {title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
                    {title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
                    {title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
                    {title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
                    {title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
                    {title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
                    {title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
                    {title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 1},
                    {title: '某某专家检测出最新需要整改的安全隐患，请查看并尽快整改提交',status: 0},
                ],

                professorMessageList: [
                    {title: '某某企业提交了最新整改的安全隐患，请查看并尽快核实',status: 1},
                    {title: '某某企业提交了最新整改的安全隐患，请查看并尽快核实',status: 1},
                    {title: '某某企业提交了最新整改的安全隐患，请查看并尽快核实',status: 1},
                    {title: '某某企业提交了最新整改的安全隐患，请查看并尽快核实',status: 1},
                    {title: '某某企业提交了最新整改的安全隐患，请查看并尽快核实',status: 1},
                    {title: '某某企业提交了最新整改的安全隐患，请查看并尽快核实',status: 1},
                    {title: '某某企业提交了最新整改的安全隐患，请查看并尽快核实',status: 0},
                    {title: '某某企业提交了最新整改的安全隐患，请查看并尽快核实',status: 1},
                    {title: '某某企业提交了最新整改的安全隐患，请查看并尽快核实',status: 1},
                    {title: '某某企业提交了最新整改的安全隐患，请查看并尽快核实',status: 1},
                ]
            }
        },
        methods: {

            //切换排序方式
            changeOrderByItem:function (e) {
                let dataset = e.currentTarget.dataset;
                this.currentOrderByItem = parseInt(dataset.index);
            },


            //查看消息详情
            viewMessageDetail:function (e) {
                let dataset = e.currentTarget.dataset;
                // 根据index获取点击项目
                let item = this.messageList[dataset.index];
                uni.navigateTo({
                    url: '../index/messageDetail'
                })
            },


            //登出
            logout:function () {
                // console.log(e)
                uni.removeStorageSync("user_info");
                uni.navigateTo({
                    url: '../me/login'
                })
            }

        },
        onShow() {
            let userInfo = uni.getStorageSync("user_info");
            if(userInfo === undefined || userInfo === null || userInfo === ''){
                this.userInfo = { user_name: '', type: ''  };
            }
            else{
                this.userInfo = userInfo;
            }
        },
    }
</script>

<style>
    page{
        background-color: #f8f8f8;
    }

    .top_box{
        width: 100%;
        position: relative;
        border-radius: 0 0 30upx 30upx;
        overflow: hidden;
    }

    .top_box .message_main_background{
        width: 100%;
        height: 400upx;
        display: block;
    }

    .top_box .top_box_content{
        position: absolute;
        left: 0;
        right: 0;
        top: 0;
        bottom: 0;
        display: flex;
        flex-flow: column;
        align-items: center;
        justify-content: space-between;
    }

    .top_box .top_box_content .hello{
        width: 100%;
        height: 250upx;
        flex: auto;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 36upx;
        color: #ffffff;
        font-weight: bold;
    }

    .top_box .top_box_content .order_by_box{
        width: 690upx;
        height: fit-content;
        margin: 0 0 40upx 0;
        display: flex;
        flex-flow: column;
        align-items: flex-start;
        justify-content: center;
    }

    .top_box .top_box_content .order_by_box .title{
        font-size: 36upx;
        color: #ffffff;
        font-weight: bold;
        margin: 0 0 20upx 20upx;
    }

    .top_box .top_box_content .order_by_box .order_by_list{
        width: 100%;
        height: 80upx;
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: #ffffff;
        border-radius: 20upx;
    }

    .top_box .top_box_content .order_by_box .order_by_list .item{
        width: 30upx;
        flex: auto;
        text-align: center;
        font-size: 30upx;
        color: #888888;
        border-right: 1px solid #2ac080;
    }

    .top_box .top_box_content .order_by_box .order_by_list .item.active{
        color: #2ac080;
    }

    .top_box .top_box_content .order_by_box .order_by_list .item:last-child{
        border-right: unset;
    }


    /* 消息列表 */
    .message_list{
        width: 100%;
    }

    .message_list .item{
        width: 690upx;
        box-sizing: border-box;
        padding: 20upx;
        display: flex;
        align-items: center;
        justify-content: space-between;
        background-color: #ffffff;
        margin: 20upx auto;
        box-shadow: 0 0 20upx 0 rgba(0,0,0,0.1);
        border-radius: 20upx;
    }

    .message_list .item .picture{
        width: 80upx;
        height: 80upx;
        background-size: cover;
        background-position: center center;
        background-repeat: no-repeat;
        margin-right: 20upx;
        border-radius: 10upx;
        overflow: hidden;
    }

    .message_list .item .picture .cover{
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .message_list .item .picture .cover.danger{
        background-color: rgba(255,0,0,0.1);
    }

    .message_list .item .picture .cover.success{
        background-color: rgba(0,255,0,0.1);
    }

    .message_list .item .picture .cover .icon{
        width: 40upx;
        height: 40upx;
        display: block;
    }

    .message_list .item .title{
        width: 50%;
        flex: auto;
        font-size: 25upx;
        color: #888888;
    }

    .message_list .item .button{
        width: fit-content;
        height: fit-content;
        display: block;
        padding: 15upx;
        background-color: #23b574;
        color: #ffffff;
        font-size: 25upx;
        line-height: 100%;
        border-radius: 10upx;
    }

    .message_list .item .button:active{
        opacity: .7;
    }

    /*没有消息*/
    .no_item_box{
        width: 690upx;
        height: 500upx;
        display: flex;
        flex-flow: column;
        align-items: center;
        justify-content: center;
        background-color: #ffffff;
        /*box-shadow: 0 0 20upx 0 rgba(41, 192, 125,0.2);*/
        margin: 40upx auto;
        border-radius: 20upx;
    }

    .no_item_box .icon{
        width: 200upx;
        height: 200upx;
    }

    .no_item_box .text{
        color: #29c07d;
        font-size: 30upx;
    }

</style>
