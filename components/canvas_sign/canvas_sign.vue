<!-- 签名绘制组件 -->
<!-- Created by Hanawa Hinata on 2020/04/26 -->

<template>
    <view v-if="visibleSync" class="canvas_sign_box" :class="{'visible':show}"
          @touchmove.stop.prevent="moveHandle">
        <view class="mask" @tap="close" />
        <view class="content">
            <canvas class='canvas_content' :canvas-id="canvasId" @touchmove='move' @touchstart='start($event)' @touchend='end'
                    @touchcancel='cancel' @longtap='tap' disable-scroll='true' @error='error' />
            <view class="btn_box">
                <view class="item" @tap="clear">清除</view>
                <view class="item" @tap="save">保存</view>
            </view>
        </view>
    </view>
</template>

<script>
    var content = null;
    var touchs = [];
    var canvasw = 0;
    var canvash = 0;
    //获取系统信息
    uni.getSystemInfo({
        success: function(res) {
            canvasw = res.windowWidth;
            canvash = canvasw * 9 / 16;
        },
    })
    export default{
        name:'canvas_sign',
        props:{
            visible: {
                type: Boolean,
                default: false
            },
            canvasId:{
                type: String,
                default: 'firstCanvas'
            }
        },
        data(){
            return{
                show:false,
                visibleSync: false,
                signImage:'',
                hasDh:false,
            }
        },
        watch:{
            visible(val) {
                this.visibleSync = val;
                this.show = val;
                this.getInfo()
            }
        },

        created(options) {
            this.visibleSync = this.visible
            this.getInfo()
            setTimeout(() => {
                this.show = this.visible;
            }, 100)
        },
        methods:{
            getInfo(){
                //获得Canvas的上下文
                content = uni.createCanvasContext(this.canvasId,this)
                //设置线的颜色
                content.setStrokeStyle("#000")
                //设置线的宽度
                content.setLineWidth(5)
                //设置线两端端点样式更加圆润
                content.setLineCap('round')
                //设置两条线连接处更加圆润
                content.setLineJoin('round')
            },
            //
            close() {
                this.show = false;
                this.hasDh = false;
                this.$emit('close')
            },
            moveHandle(){

            },
            // 画布的触摸移动开始手势响应
            start(e){
                let point = {
                    x: e.touches[0].x,
                    y: e.touches[0].y,
                }
                touchs.push(point);
                this.hasDh = true
            },
            // 画布的触摸移动手势响应
            move: function(e) {
                let point = {
                    x: e.touches[0].x,
                    y: e.touches[0].y
                }
                touchs.push(point)
                if (touchs.length >= 2) {
                    this.draw(touchs)
                }
            },

            // 画布的触摸移动结束手势响应
            end: function(e) {
                //清空轨迹数组
                for (let i = 0; i < touchs.length; i++) {
                    touchs.pop()
                }

            },

            // 画布的触摸取消响应
            cancel: function(e) {
                // console.log("触摸取消" + e)
            },

            // 画布的长按手势响应
            tap: function(e) {
                // console.log("长按手势" + e)
            },

            error: function(e) {
                // console.log("画布触摸错误" + e)
            },

            //绘制
            draw: function(touchs) {
                let point1 = touchs[0]
                let point2 = touchs[1]
                // console.log(JSON.stringify(touchs))
                content.moveTo(point1.x, point1.y)
                content.lineTo(point2.x, point2.y)
                content.stroke()
                content.draw(true);
                touchs.shift()

            },
            //清除操作
            clear: function() {
                //清除画布
                content.clearRect(0, 0, canvasw, canvash)
                content.draw(true)
                // this.close()
                this.hasDh = false;
                this.$emit('clear')
            },
            save(){
                var that = this;
                if(!this.hasDh){
                    uni.showToast({title:'请先签字',icon:'none'})
                    return;
                }
                uni.showLoading({title:'生成中...',mask:true})
                setTimeout(()=>{
                    uni.canvasToTempFilePath({
                        canvasId: this.canvasId,
                        success: function(res) {
                            that.signImage = res.tempFilePath;
                            that.$emit('save',res.tempFilePath);
                            uni.hideLoading()
                            that.hasDh = false;
                            that.show = false;
                        },
                        fail:function(err){
                            console.log(err)
                            uni.hideLoading()
                        }
                    },this)
                },100)
            }
        }
    }
</script>

<style>
    .canvas_sign_box.visible {
        visibility: visible
    }

    .canvas_sign_box {
        display: block;
        position: fixed;
        z-index: 100;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        overflow: hidden;
        height: 100vh;
        visibility: hidden;
        animation: scareShow 200ms ease-in-out;
    }

    .canvas_sign_box .mask {
        display: block;
        opacity: 0;
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, .5);
        transition: opacity .3s;
    }

    .canvas_sign_box .content {
        display: block;
        position: absolute;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        margin: auto;
        width: 94%;
        height: fit-content;
        background: #f8f8f8;
        border-radius: 8upx;
        overflow: hidden;
        box-shadow: 0 0 20upx 0 rgba(0,0,0,0.2);
    }

    /*canvas*/
    .canvas_sign_box .canvas_content {
        background-color: #fff;
        width: 100%;
        height: 400upx;
    }

    /*canvas*/

    .canvas_sign_box .btn_box {
        padding: 0;
        height: 100upx;
        overflow: hidden;
        margin: auto;
        display: flex;
        justify-content: space-between;
    }

    .canvas_sign_box .item {
        width: 50%;
        height: 100upx;
        flex: auto;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 36upx;
        color: #353535;
        border-radius: 6upx;
        border-right: 1px solid #d9d9d9;
    }

    .canvas_sign_box .item:last-child{
        border-right: unset;
    }


    .visible .mask {
        display: block;
        opacity: 1
    }
</style>