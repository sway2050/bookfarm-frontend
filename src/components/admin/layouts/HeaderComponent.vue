<template>
    <header>
        <nav class="square-form navbar navbar-expand-lg nav-color">
            <div class="container-fluid">
                <a class="navbar-brand" href="#">Book Farm - Let Your Knowledge Grow</a>
                <button class="navbar-toggler" type="button" data-mdb-toggle="collapse"
                    data-mdb-target="#navbarSupportedContent" aria-controls="navbarSupportedContent"
                    aria-expanded="false" aria-label="Toggle navigation">
                    <i class="fas fa-bars"></i>
                </button>
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item text-right">
                            <!-- <a class="nav-link active" href="javascript:void(0);" @click="logout()">Logout</a> -->
                            <button type="button" class="btn btn-outline-dark" @click="logout()">Logout</button>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </header>
</template>
<script>
export default {
    props: ['appName'],
    methods: {
        async logout() {
            this.store.dispatch('pageLoader', { show: true })
            var response = await this.adminApi('GET', 'logout');
            this.store.dispatch('pageLoader', { show: false })

            if (response) {
                localStorage.removeItem('admin_access_token')
                setTimeout(() => {
                    this.$router.push({ name: 'admin.login' });
                }, 1000);
            }
        }
    },
}
</script>