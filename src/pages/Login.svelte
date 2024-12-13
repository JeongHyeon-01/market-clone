<script>
    import { getAuth, signInWithPopup, GoogleAuthProvider } from "firebase/auth";
    import { user$ } from "../store";
    const provider = new GoogleAuthProvider();
    const auth = getAuth();
    

    const loginWithGoogle = async() => {
        try{
            const result = await signInWithPopup(auth, provider)
            const credential = GoogleAuthProvider.credentialFromResult(result);
            const token = credential.accessToken;
            const user = result.user;
            user$.set(user)
            localStorage.setItem('token', token)
        }
        catch(error) {
            // Handle Errors here.
            const errorCode = error.code;
            const errorMessage = error.message;
            // The email of the user's account used.
            const email = error.customData.email;
            // The AuthCredential type that was used.
            const credential = GoogleAuthProvider.credentialFromError(error);
            // ...
            console.error(errorCode, errorMessage)
        };
    }
    

</script>

<div>
    <div>로그인하기</div>
    <button class="login-btn" on:click={loginWithGoogle}>
        <img class="google-img" src="data:image/png;base64,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" alt="">   
        <div>Google로 시작하기</div>
    </button>
</div>


<style>
    .login-btn {
        height: 50px;
        width: 200px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        border: 1px solid gray;
        cursor: pointer;
        border-radius: 3px;
    }
    .google-img{
        width: 50px;
    }
</style>