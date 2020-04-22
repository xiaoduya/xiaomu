<template>
    <div class="login_container">
        <div class="log_box">
            <div class="avatar_box">
                <img src="./../assets/logo.png">
            </div>
            <!-- 表单提交区域 -->
            <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
                <!-- 用户名 -->
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
                </el-form-item>
                <!-- 密码 -->
                <el-form-item prop="password">
                    <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
                </el-form-item>
                <!-- 按钮区域 -->
                <el-form-item>
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="loginFormRef">重置</el-button>

                </el-form-item>
            </el-form>
        </div>
    </div>
</template>
<script>
export default {
    name : 'login',
    data(){
        return {
            loginForm : {
                username : '',
                password : ''
            },
            loginFormRules : { //表单验证规则
                //验证用户名 
                username : [
                    {required: true,message: '请输入用户名',trigger: 'blur'},
                    {min: 3,max: 10,message: '长度在 3 到 10个字符之间',trigger: 'blur'}
                    
                ],
                password : [
                     {required: true,message: '请输入密码',trigger: 'blur'},
                    {min: 6,max: 15,message: '长度在 6 到 15个字符之间',trigger: 'blur'}
                ]
            }   
        }
    },
    methods: {
        //点击重置按钮 重置登录表单
        loginFormRef(){
          this.$refs.loginFormRef.resetFields()//resetFields重置函数
        },
        login(){// 表单预验证
            this.$refs.loginFormRef.validate(async valid => {//validate 预验证函数 接受一个形参
                if (!valid) return;
                const {data: res} = await this.$http.post('login', this.loginForm)
                if (res.meta.status !== 200) return this.$message.error('登录失败')
                this.$message.success('登录成功')
                //设置session保存 token 登录状态
                window.sessionStorage.setItem('token',res.data.token)
                this.$router.push('/home')
            })
        }
    }
}
</script>
<style lang="stylus" scoped>
.login_container{
    background-color #2b4b6b
    height 100%;
    .log_box{
        position absolute
        top 50%
        left 50%
        transform translate(-50%,-50%)
        width 450px
        height 300px
        background-color #fff
        border-radius 5px
        .login_form{
            position absolute
            bottom 0
            width 100%
            padding 0 10px
            box-sizing border-box
        }
        .avatar_box{
            position absolute
            top 50%
            left 50%
            transform translate(-50%,-150%)
            width 130px
            height 130px
            border-radius 50%
            padding 10px
            box-shadow 0 0 10px #ccc
            img{
                width 100%
                height 100
                border-radius 50%
                background-color #eee
            }
        }
    }
}
</style>