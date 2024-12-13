<script>
  import Nav from "../components/nav.svelte";
  import { user$ } from "../store";
    const logout = ()=> {
        user$.set(null);
        localStorage.removeItem('token');
    }
</script>
<div>나의 당근</div>
<button on:click={logout}>로그아웃</button>
<Nav location="my"/>


<style>
    button {
        width: 200px;
        height: 50px;
        cursor: pointer;
        border: 1px solid rgba(0, 0, 0, 0.3);
    }
</style>