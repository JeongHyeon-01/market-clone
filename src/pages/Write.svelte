<script>
    import { getStorage, ref as refs, uploadBytes, getDownloadURL } from "firebase/storage";
    import { getDatabase, ref, push } from "firebase/database";
    import Nav from "../components/nav.svelte";
    // 바인딩될 변수 초기화
    let title = "";
    let price = "";
    let description = "";
    let place = "";
    let files;
    let url = "";

    const db = getDatabase();
    const storage = getStorage();
    $: if(files) console.log(files[0])

    // 데이터를 Firebase로 전송하는 함수
    function writeProductData(event,url) {
        // Firebase로 전송할 데이터
        const productData = {
            title: title || "",
            price: parseInt(price) || 0,
            description: description || "",
            place: place || "",
            insertAt: new Date().getTime(),
            images : url || ""
        };

        console.log("전송 데이터:", productData); // 디버깅용

        // Firebase에 데이터 전송
        push(ref(db, 'products/'), productData)
            .then(() => {
                alert("데이터가 성공적으로 저장되었습니다!");
                // 필드 초기화
                title = "";
                price = "";
                description = "";
                place = "";
                url = "";
            })
            .catch((error) => {
                alert("데이터 저장 중 오류 발생: " + error.message);
            });
            alert("등록완료");
            window.location.hash ="/";
        };
        


        // // 'file' comes from the Blob or File API
        // uploadBytes(storageRef, file).then((snapshot) => {
        // console.log('Uploaded a blob or file!');
        // });
        const uploadFile = async() => {
            const file = files[0]
            const name = file.name;
            const imageRef = refs(storage, name)
            const res = await uploadBytes(imageRef, file);
            const url = await getDownloadURL(imageRef);
            return url;
        }
        const handelSubmit = async(event) => {
            const url = await uploadFile()
            writeProductData(event, url)

        }
</script>
<form id="write-form" on:submit|preventDefault={handelSubmit}>
    <div>
        <label for="image">이미지</label>
        <input type="file" bind:files id="image" name="image" />
      </div>
    <div>
        <label for="title">제목</label>
        <input type="text" id="title" name="title" bind:value={title} />
    </div>
    <div>
        <label for="price">가격</label>
        <input type="number" id="price" name="price" bind:value={price} />
    </div>
    <div>
        <label for="description">설명</label>
        <input type="text" id="description" name="description" bind:value={description} />
    </div>
    <div>
        <label for="place">장소</label>
        <input type="text" id="place" name="place" bind:value={place} />
    </div>
    <div>
        <button class="submit-btn" type="submit">글쓰기 완료</button>
    </div>
</form>

<Nav location="write"/>

<style>
    .submit-btn {
        background-color: tomato;
        border-radius: 10px;
        padding: 5px 12px 5px 12px;
        color: white;
        cursor: pointer;
    }

</style>