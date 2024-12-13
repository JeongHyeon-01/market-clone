
<script>
  import Signup from "./pages/Signup.svelte";
  import Login from "./pages/Login.svelte";
  import Write from "./pages/Write.svelte";
  import Router from "svelte-spa-router";
  import Notfound from "./pages/Notfound.svelte";
  import "./css/style.css";
  import { GoogleAuthProvider,signInWithCredential,getAuth } from "firebase/auth";
  import { user$ } from "./store";
  import { onMount } from "svelte";
  import Loading from "./pages/Loading.svelte";
  import MyPage from "./pages/MyPage.svelte";
  import Nav from "./components/nav.svelte";
  import Main from "./pages/Main.svelte";
  
  
  let isLoading = true;
  const auth = getAuth();

  const checkLogin = async() => {
    const token = localStorage.getItem('token')
    if (!token){
      isLoading=false;
      return 
    };
    const credential = GoogleAuthProvider.credential(null,token);
    const result = await signInWithCredential(auth, credential)
    const user = result.user;
    user$.set(user)
    isLoading = false;
  }
  
  const routes = {
    "/" :   Main,
    "/signup" : Signup,
    "/write" : Write,
    "/my" : MyPage,
    "*" : Notfound
  }

  onMount(()=>checkLogin());
</script>
{#if isLoading}
  <Loading/>
{:else if !$user$}
  <Login/>
{:else}
  <Router {routes}/>
{/if}
