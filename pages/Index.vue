<template>
    <div>
        <div class="content">
            <p v-if="users" id="userId">Hi {{users}} <a class="link" v-on:click="logout()" href="javascript:void(0)">注销</a></p>
            <p v-else><button v-on:click="toLogin()" class="button">&nbsp; 登录 &nbsp;</button></p>
            <p id="locationId"></p>
            <div>
                <p><button class="button" v-on:click="getLocation()">刷新定位信息</button></p>
                <p>
                    <span>上传图片</span>
                    <input type="file" name="image" accept=“image/*” v-on:change='handleInputChange()'/>
                </p>
            </div>
        </div>
    </div>
</template>

<script>
import {mapActions} from 'vuex';
import Vue from 'vue';

function setState(store) {
    store.dispatch('appShell/appHeader/setAppHeader', {
        show: true,
        title: 'PWA Demo',
        showMenu: true,
        showBack: false,
        showLogo: false,
        actions: [
            {
                icon: 'search',
                route: '/search'
            }
        ]
    });
} 

function getUser1() {
    let _user = localStorage.getItem("localUser")
    console.log("user", _user)
    if (_user) {
        return _user
    } else {
        return ''
    }
}

export default {
    name: 'index',
    data() {
        return {
            users: getUser1()
        }
    },
    metaInfo: {
        title: 'Home',
        titleTemplate: '%s - PWA Demo',
        meta: [
            {name: 'keywords', content: 'lavas PWA'},
            {name: 'description', content: '基于 Vue 的 PWA 解决方案，帮助开发者快速搭建 PWA 应用，解决接入 PWA 的各种问题'}
        ]
    },
    created: () => {
        navigator.geolocation.getCurrentPosition(function (position) {
            console.log("位置信息", position)
            document.getElementById("locationId").innerHTML = 
            "经度：" + position.coords.longitude + "<br/>纬度：" + position.coords.latitude
        })
    },
    mounted: () => {
        //console.log("be")
    },
    methods: {
        getUser() {
            let _user = localStorage.getItem("localUser")
            console.log("user", _user)
            if (_user) {
                this.user = _user
            } else {
                this.$data.users = 'Lavas'
                console.log("users", this.$data.users)
            }
        },
        getLocation() {
            console.log("开始获取位置")
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(function (position) {
                    console.log("位置信息", position)
                    document.getElementById("locationId").innerHTML = 
                    "经度：" + position.coords.longitude + "<br/>纬度：" + position.coords.latitude
                    alert("经度：" + position.coords.longitude + "\n纬度：" + position.coords.latitude)
                })
            } else {
                alert('你的浏览器不支持当前地理位置信息获取')
            }
            //this.getUser()
        },
        handleInputChange() {
            alert("上传成功")
        },
        toLogin() {
            this.$router.push("/login");
        },
        logout() {
            if (confirm("您确定要注销吗？")) {
                this.$data.users = ''
                localStorage.removeItem("localUser")
            }
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

<style lang="stylus" scoped>
    p
        width 100%
    .content
        display flex
        align-items center
        justify-content center
        height 100%
        flex-wrap wrap
    h2
        font-size 46px
        font-weight 500
    button
        background #1976d2
        padding 10px 16px
        box-shadow 0px 3px 5px rgba(19, 76, 200, .3)
        color white
    button:active
        box-shadow none
    .link
        text-decoration none 
    .link:hover
        text-decoration underline
</style>
