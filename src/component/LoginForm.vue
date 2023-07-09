<script setup>
    const emit = defineEmits({
        click: null,
        submit:({ email, password }) => {
            if( email && password ){
                return true
            }else{
                console.warn('Invalid submit event payload')
                return false
            }
        }
    })
    function subitForm( email, password ){
        emit('submit',{email,password})
    }
</script>
<template>
    <div>
        
    </div>
</template>