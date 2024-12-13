<script>
    export let location = '';
    const moveToHome = () => {
        window.location.hash = '/'
    }
    const moveToMy = () => {
        window.location.hash = '/my'
    }
</script>

<footer>
    <div class="footer-block">
    <button class="footer-icons" on:click={moveToHome}>
        <div class="footer-icons__img">
            {#if location === "home"}
                <img src="./assets/home-fill.svg" alt="" />
                
            {:else}
                <img src="./assets/home-empty.svg" alt="" />
            {/if}
        </div>
        <div class="footer-icons__disc">홈</div>
    </button>
    <div class="footer-icons">
        <div class="footer-icons__img">
        <img src="./assets/newspaper-empty.svg" alt="" />
        </div>
        <div class="footer-icons__disc">동네생활</div>
    </div>
    <div class="footer-icons">
        <div class="footer-icons__img">
        <img src="./assets/location-empty.svg" alt="" />
        </div>
        <div class="footer-icons__disc">내 근처</div>
    </div>
    <div class="footer-icons">
        <div class="footer-icons__img">
        <img src="./assets/chat-empty.svg" alt="" />
        </div>
        <div class="footer-icons__disc">채팅</div>
    </div>
    <button class="footer-icons" on:click={moveToMy}>
        <div class="footer-icons__img">
        {#if location === "my"}
            <img src="./assets/user-fill.svg" alt="" />
        {:else}
            <img src="./assets/user-empty.svg" alt="" />
        {/if}
        </div>
        <div class="footer-icons__disc">나의 당근</div>
    </button>
    </div>
</footer>