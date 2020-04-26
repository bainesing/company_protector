<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */



    /*公共动画*/
    @keyframes upShow{
        from{
            opacity: 0;
            transform: translateY(20%);
        }
        to{
            opacity: 1;
            transform: translateY(0);
        }
    }

    @keyframes scaleShow{
        from{
            opacity: 0;
            transform: scale(1.2);
        }
        to{
            opacity: 1;
            transform: scale(1);
        }
    }


    @keyframes hideShow{
        from{
            opacity: 0;
        }
        to{
            opacity: 1;
        }
    }


    @keyframes slideShow{
        from {
            transform: translateX(50%);
            opacity: 0;
        }

        to {
            transform: translateX(0);
            opacity: 1;
        }
    }

    @keyframes loadBackground {
        0%{
            background-color: rgba(0,0,0,0.5);
        }
        50%{
            background-color: rgba(0,0,0,0.2);
        }
        100%{
            background-color: rgba(0,0,0,0.5);
        }
    }
</style>
