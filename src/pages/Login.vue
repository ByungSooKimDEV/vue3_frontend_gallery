<template>
    <main class="form-signin w-100 m-auto">
        <h1 class="h3 mb-3 fw-normal">Please sign in</h1>

        <div class="form-floating">
            <input type="email" class="form-control" id="floatingInput" placeholder="name@example.com" @keyup.enter="submit()" v-model="state.form.email" />
            <label for="floatingInput">Email address</label>
        </div>
        <div class="form-floating">
            <input type="password" class="form-control" id="floatingPassword" placeholder="Password" @keyup.enter="submit()" v-model="state.form.password" />
            <label for="floatingPassword">Password</label>
        </div>

        <div class="checkbox mb-3">
            <label> <input type="checkbox" value="remember-me" @keyup.enter="submit()" /> Remember me </label>
        </div>
        <button class="w-100 btn btn-lg btn-primary" @click="submit()">Sign in</button>
        <p class="mt-5 mb-3 text-muted">&copy; 2017–2022</p>
    </main>
</template>

<script>
import store from '@/scripts/store';
import router from '@/scripts/router';
import axios from 'axios';
import { reactive } from 'vue';
export default {
    name: 'Login',
    setup() {
        const state = reactive({
            form: {
                email: '',
                password: '',
            },
        });

        const submit = () => {
            axios
                .post('/api/account/login', state.form)
                .then((res) => {
                    store.commit('setAccount', res.data);
                    // 새로고침을 하면 로그인이 풀림 -> backend에서 세션 관리하고, client에서 토큰 관리를 할 수 있다.
                    // 일단은 sessionStorage를 사용하자...
                    sessionStorage.setItem('id', res.data);
                    router.push({ path: '/' });
                    window.alert('로그인하였습니다.');
                })
                .catch(() => {
                    window.alert('로그인 정보가 존재하지 않습니다.');
                });
        };

        return { state, submit };
    },
};
</script>
<style scoped>
.form-signin {
    max-width: 330px;
    padding: 15px;
}

.form-signin .form-floating:focus-within {
    z-index: 2;
}

.form-signin input[type='email'] {
    margin-bottom: -1px;
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 0;
}

.form-signin input[type='password'] {
    margin-bottom: 10px;
    border-top-left-radius: 0;
    border-top-right-radius: 0;
}
</style>
