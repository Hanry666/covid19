<template>
    <div class="box">
        <div class="title">登录</div>
        <el-input class="user input" placeholder="请输入账号" v-model="userid" clearable></el-input>
        <div class="password input"> <el-input class="password" placeholder="请输入密码" v-model="password"
                show-password></el-input></div>
        <div class="change">
            <el-button type="warning" class="reg" @click="ChangeReg">切换注册</el-button>
            <el-button type="primary" class="but" @click="login">提交登录</el-button>
        </div>

    </div>
</template>
<script>
import throttle from 'lodash/throttle'  //引入节流函数
export default {
    name: '',
    data() {
        return {
            password: '',
            userid: ''
        }
    },
    mounted() {
        window.addEventListener('keydown', (e) => {  //键盘监听事件
            if (!this.$store.state.bar.IsShowLog)  //只有在注册登录界面时才会触发
                return;
            if (e.key === 'Enter') {   //当按下enter键时提交表单
                if (!this.$store.state.bar.Isreg) {  //当处于登录界面时
                    this.login();
                }
            }
        })
    },
    methods: {
        ChangeReg() {
            this.$store.commit('bar/CHANGEREG');
        },
        login: throttle(async function () {  //登录  节流
            try {
                await this.$store.dispatch('myself/Login',{name:this.userid,password:this.password});   
                this.$store.commit('bar/SHOWLOG');  //关闭登录界面
                //置空表单
                this.password = '';
                this.userid = '';
                this.$element.Message({message: '登录成功',type: 'success'});
            } catch (err) {
                //置空表单
                this.password = '';
                this.userid = '';
                this.$element.Message.error(err.message);
            }
        }, 3000),
    }
}
</script>
<style lang="css" scoped>
.box {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}

.title {
    font-size: 2.125rem;
    FILTER: glow(strength=4)mask(color=#E1E4EC)
}

.input {
    width: 70%;
    margin-top: 3.25rem;
}

.change {
    margin-top: 3rem;
    margin-bottom: 2rem;
    width: 70%;

}

.reg {
    width: 45%;

}

.but {
    width: 45%;
    float: right;
}

.password {}
</style>