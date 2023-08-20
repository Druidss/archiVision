<!-- src/ThreeComponent.svelte -->
<script>
     const modelImage = "./src/assets/placeholderModel.png"; // 占位图片路径
</script>

<style>
    .three-container {  
        max-height: 100%; /* 确保图片不超出容器的高度 */
        margin-top: 5rem;

    }

    .model-image {
        /* width: 100%; */
        height: 100%;
        object-fit: contain;
    }
</style>

<div class="three-container">
    <img class="model-image" src={modelImage} alt="3D Model Placeholder">
</div>
