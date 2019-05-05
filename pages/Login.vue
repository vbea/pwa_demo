<template>
    <form action="/" class="container" v-on:submit="onLogin(this)">
        <br/>
        <input type="text" v-model="username" placeholder="请随便输入用户名" required="required" />
        <br/>
        <input type="password" id="password" placeholder="请随便输入密码" required="required" />
        <button type="submit" class="button" >登录</button>
    </form>
</template>
<script>
import {mapActions} from 'vuex';
function setState(store) {
    store.dispatch('appShell/appHeader/setAppHeader', {
        show: true,
        title: 'Login',
        showMenu: false,
        showBack: true,
        showLogo: false,
        actions: [
            {
                icon: 'home',
                route: '/index'
            }
        ]
    });
} 
function getUser1() {
    let _user = localStorage.getItem("localUser")
    if (_user) {
        return _user
    } else {
        return ''
    }
}
export default {
    name: "login",
    data() {
        return {
            username: getUser1()
        }
    },
    metaInfo: {
        title: 'Login',
        titleTemplate: '%s - PWA Demo'
    },
    methods: {
        onLogin() {
            localStorage.setItem("localUser", this.$data.username)
        }
    },
    async asyncData({store, route}) {
        setState(store);
    },
    activated() {
        setState(this.$store);
    }
};
</script>
<style>
    .container {
        padding: 16px;
    }
    .container input {
        height: 34px;
        width: 100%;
        outline: none;
        padding: 5px;
        font-size: 16px;
        margin-bottom: 16px;
        border-bottom: 1px solid #ccc;
    }
    .container input:focus {
        transition: all .4s;
        border-bottom: 1px solid #1976d2;
        box-shadow: 0 1px 0 0 #1976d2;
    }
    .container button {
        width: 100%;
        background: #1976d2;
        padding: 10px 16px;
        box-shadow: 0px 3px 5px rgba(19, 76, 200, .3);
        color: white;
    }
</style>
