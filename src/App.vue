<template>
    <div>
        <Header />
        <RouterView />
        <Footer />
    </div>
</template>

<script>
import axios from 'axios';
import Footer from './components/Footer.vue';
import Header from './components/Header.vue';
import store from './scripts/store';
import { useRoute } from 'vue-router';
import { watch } from 'vue';

export default {
    name: 'App',
    components: { Header, Footer },
    setup() {
        // // sessionStorage에서 id 값을 가져온 다음
        // const id = sessionStorage.getItem('id');
        // // id 값이 있다면 store에 commit을 하고 id 값을 넣어준다.
        // if (id) {
        //     store.commit('setAccount', id);
        // }
        const check = () => {
            axios.get('/api/account/check').then(({ data }) => {
                console.log(data);
                store.commit('setAccount', data || 0);
            });
        };

        const route = useRoute();

        // 경로가 바뀔때마다 token 값 체크하도록한다.
        watch(route, () => {
            check();
        });
    },
};
</script>

<style>
.bd-placeholder-img {
    font-size: 1.125rem;
    text-anchor: middle;
    -webkit-user-select: none;
    -moz-user-select: none;
    user-select: none;
}

@media (min-width: 768px) {
    .bd-placeholder-img-lg {
        font-size: 3.5rem;
    }
}

.b-example-divider {
    height: 3rem;
    background-color: rgba(0, 0, 0, 0.1);
    border: solid rgba(0, 0, 0, 0.15);
    border-width: 1px 0;
    box-shadow: inset 0 0.5em 1.5em rgba(0, 0, 0, 0.1), inset 0 0.125em 0.5em rgba(0, 0, 0, 0.15);
}

.b-example-vr {
    flex-shrink: 0;
    width: 1.5rem;
    height: 100vh;
}

.bi {
    vertical-align: -0.125em;
    fill: currentColor;
}

.nav-scroller {
    position: relative;
    z-index: 2;
    height: 2.75rem;
    overflow-y: hidden;
}

.nav-scroller .nav {
    display: flex;
    flex-wrap: nowrap;
    padding-bottom: 1rem;
    margin-top: -1px;
    overflow-x: auto;
    text-align: center;
    white-space: nowrap;
    -webkit-overflow-scrolling: touch;
}
</style>
