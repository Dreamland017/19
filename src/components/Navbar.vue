<template>
    <nav>
        <div class="nav-container">
            <a href="index.html">
                <img src="imgs/DBook.png" class="logonav">
            </a>

            <div class="nav-profile">
                <p class="nav-profile-name">Cart</p>
                <div onclick="openCart()" style="cursor: pointer;" class="nav-profile-cart">
                    <i class="fas fa-cart-shopping"></i>
                    <div id="cartcount" class="cartcount" style="display: none;">
                        0
                    </div>
                </div>
            </div>
        </div>
    </nav>
</template>

<script>
export default {

}
</script>

<style>

</style>