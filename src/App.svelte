<!-- src/app.svelte -->
<script>
    import ThreeComponent from './ThreeComponent.svelte';
    import { onMount } from 'svelte';
    import { client } from "@gradio/client";
    import { imageData } from './image.js';
    

    let app_info;
    const url = 'https://3ec67e35ce3ecd1447.gradio.live'

    onMount(async () => {
        let app = await client(url);
        app_info = await app.view_api();
        console.log(app_info);
        console.log(typeof(imageData.base64Image));
       const result = await app.predict("/upload", [imageData.base64Image,"Howdy!"]);
       // this is the problem
        console.log(result);
  });

   

    const logoSizeRem = "4rem"; // 64px converted to rem
    const logoMarginRem = "9rem"; // 145px converted to rem
    const logoText = "ArchiVision AI";
    const taglineText = "AI-Powered 3D Building";

    const sloganText = "Image to 3D Vision";
    const explanationText = "Transforming Images into 3D Realities: Upload photos of architecture and witness our AI decipher their essence, crafting intricate 3D models from AI's visions.";
    const sloganWidthRem = "31.25rem"; // 500px converted to rem
    const explanationWidthRem = "28.125rem"; // 450px converted to rem
    const buttonText = "Upload Image";
    const buttonIconSizeRem = "1.5rem"; // 24px converted to rem
    const buttonTextSizeRem = "1.25rem"; // 20px converted to rem
    const threeMarginRem = "6.25rem"; // 100px converted to rem

    let logoSrc = "./src/assets/logo.png"
    let modelImage = "./src/assets/placeholderModel.png"; // 占位图片路径
    let base64Image = "";
    let fileUploaded = false;

    function handleImageUpload(event) {
        let input = document.querySelector('#input');
        // @ts-ignore
        input.click();
        const file = event.target.files[0]; // 获取用户选择的文件
        fileUploaded = true;
        if (file) {
        const reader = new FileReader();
        reader.onload = () => {
            // @ts-ignore
            base64Image = reader.result; // 将读取到的数据设置为 base64Image
        };
        reader.readAsDataURL(file); // 读取文件并转换为 base64 编码
        console.log("ok");
        }
  }
</script>

<style>
    :global(body) {
        margin: 0;
        padding: 0;
        font-family: Arial, sans-serif;
    }
    button{
        border:none;
    }

    .container {
        display: flex;
        align-items: center;
        height: 100vh;
        background: url('./src/assets/background.png') no-repeat center center fixed;
        background-size: cover;
        padding-right: 2.5rem; /* 40px converted to rem  containerPaddingRem */
        padding-left: 2.5rem; /* 40px converted to rem  containerPaddingRem */
        justify-content: space-between;
    }

    .container-left,
    .container-right {
        flex: 1;
    }

    .container-left{
        max-width: 50vw;
        height: 100vh;
        margin-left: 5rem;
    }
    .container-right{
        max-width: 50vw;
        height: 100vh;
    }

    .container-logo {
        height: 4rem;
        display: flex;
        justify-content:  flex-start;
        margin-top: 2rem;  /* 距离页面顶部的距离 */
    }


    .logo {
        width: 4rem;
        height: 3rem;
        margin-right: 0rem;
    }

    .text-container {
        display: flex;
        flex-direction: column;
        gap: 1.25rem; /* 20px converted to rem */
    }

    .logoText {
        font-size: 1.5rem; /* 24px converted to rem */
        font-weight: bold;
    }
    .tagline{
        font-size: 0.6rem;
    }



    .slogan {
        margin-top: 1rem;
        font-size: 7rem; /* 12px converted to rem */
        color: #828282;
    }

    .slogan,
    .explanation {
        max-width: 31.25rem;
    }

    .slogan {
        margin-top: 0.75rem; /* 12px converted to rem */
        color: black;
        width: 31.25rem;
    }
    
    .explanation{
        font-size: 1rem; /* 16px converted to rem */
        color: #828282;
        width: 28.125rem;
        margin-top: -1rem;
        margin-block-end: 2rem;
    }
    
    .text-container {
        display: flex;
        flex-direction: column;
        gap: 1.25rem; /* 20px converted to rem */
    }

    .upload-button {
        display: flex;
        align-items: center;
        width: 28rem;
        background-color: #37C9FF;
        padding: 0.625rem; /* 10px converted to rem */
        border-radius: 0.3125rem; /* 5px converted to rem */
        color: white;
        cursor: pointer;
        margin-right: 1rem;
    }

    .upload-icon {
        width: 1.5rem;
        height: 1.5rem;
        margin-left: 8rem;
        margin-right: 1rem; /* 20px converted to rem */
    }

    .button-text {
        font-size: 1.25rem;
    }

    .container-right {
        margin-left: 6.25rem;
    }

    .base64{
        max-width: 20vw;
        max-height: 20vh;
        object-fit: contain;
    }

</style>

<div class="container">
    
    <div class="container-left">

        <div class="container-logo">
            <div class="logo">
                <img src={logoSrc} alt="Logo" class="logo">
            </div>
            <div class="text">
                <div class="logoText">{logoText}</div>
                <div class="tagline">{taglineText}</div>
            </div>

        </div>


        <div class="text-container">
            {#if base64Image}
            <img src={base64Image} alt="Uploaded Image" class="base64"/>
            {:else}
            <div class="slogan">{sloganText}</div>
            <div class="explanation">{explanationText}</div>
            {/if}

            <button class="upload-button"  on:click={handleImageUpload} >
                <img class="upload-icon" src="./src/assets/upload-icon.png" alt="Upload Icon">
                {#if !fileUploaded && !base64Image}
                <div class="button-text">{buttonText}</div>
                {:else if fileUploaded && !base64Image}
                <div class="button-text">Loading..</div>
                {:else}
                <div class="button-text">{buttonText}</div>
                 {/if}
                <input type="file" accept="image/*" onchange={handleImageUpload} id="input" style="display:none;" />

            </button>


        </div>
    </div>


    <div class="container-right">
            <ThreeComponent />
    </div>

</div>

