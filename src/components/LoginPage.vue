<template>
    <div class="uk-vertical-align text-center uk-height-1-1">
        <form class="uk-panel uk-panel-box">
            <div class="uk-form-row">
                <input
                    v-model="username"
                    class="uk-width-1-1 uk-form-large input w-auto"
                    type="text"
                    autocomplete="username"
                    :placeholder="$t('login.username')"
                    :aria-label="$t('login.username')"
                />
            </div>
            <div class="uk-form-row">
                <input
                    v-model="password"
                    class="uk-width-1-1 uk-form-large input w-auto"
                    type="password"
                    autocomplete="password"
                    :placeholder="$t('login.password')"
                    :aria-label="$t('login.password')"
                />
            </div>
            <div class="uk-form-row">
                <a class="uk-width-1-1 uk-button uk-button-large w-auto" @click="login" v-text="$t('titles.login')" />
            </div>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            username: null,
            password: null,
        };
    },
    mounted() {
        //TODO: Add Server Side check
        if (this.getAuthToken()) {
            this.$router.push("/");
        }
    },
    activated() {
        document.title = this.$t("titles.login") + " - Piped";
    },
    methods: {
        login() {
            this.fetchJson(this.apiUrl() + "/login", null, {
                method: "POST",
                body: JSON.stringify({
                    username: this.username,
                    password: this.password,
                }),
            }).then(resp => {
                if (resp.token) {
                    this.setPreference("authToken" + this.hashCode(this.apiUrl()), resp.token);
                    window.location = "/"; // done to bypass cache
                } else alert(resp.error);
            });
        },
    },
};
</script>
