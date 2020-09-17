<template>
        <nav>
            <div class="nav-wrapper">
                <router-link to="/" class="brand-logo">Задачи</router-link>
                <ul id="nav-mobile" class="right hide-on-med-and-down">
                    <router-link tag="li" to="/" exact active-class="active"><a href="#">Создать</a></router-link>
                    <router-link tag="li" to="/list" active-class="active"><a href="#">Список</a></router-link>
                </ul>
            </div>
        </nav>
</template>

<style lang="scss">
    nav{
        padding: 0 20px;
    }
</style>
