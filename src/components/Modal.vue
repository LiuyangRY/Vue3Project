<template>
    <teleport to="#teleport">
        <div id="center">
            <h2>Liuayang.com</h2>
        </div>
    </teleport>
</template>

<script lang="ts">
export default {
    
};
</script>
<style>
    #center {
        width: 200px;
        height: 200px;
        border: 2px solid black;
        background: white;
        position: fixed;
        left: 50%;
        top: 60%;
    }
</style>