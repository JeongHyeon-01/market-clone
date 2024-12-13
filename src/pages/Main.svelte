<script>
    import { getDatabase, ref, onValue } from "firebase/database";
    import Nav from "../components/nav.svelte";
    import { onMount } from "svelte";



    let hour = String(new Date().getHours()).padStart(2,"0")
    let min = String(new Date().getMinutes()).padStart(2,"0")
    

    const db = getDatabase();
    const dbRef = ref(db, 'products/');

    $: products = []
    onMount(()=> {
        onValue(dbRef, (snapshot) => {
        const data = snapshot.val()
        products = Object.values(data).reverse();
        });
    })
    const calcTime = (timestamp) => {
        const curTime = new Date().getTime() - 9*60*60*1000;
        const time = new Date (curTime - timestamp);
        const hour = time.getHours();
        const min = time.getMinutes();
        const second = time.getSeconds();
        if (hour > 0){
            return `${hour} 시간전`
        }
        else if (min > 0 ){
            return `${min}분 전`
        }
        else if (second >= 0){
            return `${second}초 전`
        }else{
            return `방금 전`
        }
    };
    
</script>
<header>
    <div class="info-bar">
    <div class="info-bar__time">{hour} : {min}</div>
    <div class="info-bar__icons">
        <img src="./assets/chart-bar.svg" alt="chart-bar" />
        <img src="./assets/wifi.svg" alt="wifi" />
        <img src="./assets/battery.svg" alt="battery" />
    </div>
    </div>
    <div class="menu-bar">
    <div class="menu-bar__location">
        <div>역삼 1동</div>
        <div class="menu-bar__location-icon">
        <img src="./assets/arrow-down.svg" alt="" />
        </div>
    </div>
    <div class="menu-bar__icons">
        <img src="./assets/search.svg" alt="" />
        <img src="./assets/menu.svg" alt="" />
        <img src="./assets/alert.svg" alt="" />
    </div>
    </div>
</header>

<main>
    {#each products as item}
    <div class="item-list">
        <div class="item-list__img">
            <img src="{item.images}" alt="{item.title}">
        </div>
        <div class="item-list__info">
            <div class="item-list__info-title">{item.title}</div>    
            <div class="item-list__info-meta">{item.place} {calcTime(item.insertAt)}</div>
            <div class="item-list__info-price">{item.price}</div>
            <div>{item.description}</div>
        </div>
    </div>
    {/each}
    <a class="write-btn" href="#/write">+ 글쓰기</a>
</main>
<Nav location="home"/>
<div class="media-info-msg">화면 사이즈를 줄여주세요.</div>
