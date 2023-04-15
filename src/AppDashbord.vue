<template>
    <div class="card card-w70" >      
        <h3 v-if="blocks.length === 0">Добавьте первый блок, чтобы увидеть результат </h3>      
        <div 
            v-else
            v-for="block in blocks"
            :key="block"
            
        > 
        
        <!-- <app-title v-if="block.blockName === 'title'">{{block.blockText}}</app-title>
        <app-subtitle v-if="block.blockName === 'subtitle'">{{block.blockText}}</app-subtitle>
        <app-avatar v-if="block.blockName === 'avatar'" :src="block.blockText"></app-avatar>
        <app-text v-if="block.blockName === 'text'">{{block.blockText}}</app-text> -->
        <component 
            :is="'app-' + block.blockName" 
            v-if="block.blockName === 'avatar'" 
            :src="block.blockText">
        </component>
        
        <component :is="'app-' + block.blockName" v-else>
            {{block.blockText}}
        </component>

        </div>
        <h3 v-if="isError">Нет данних на сервере</h3>
    </div> 
</template>
  
<script>
import AppTitle from './AppTitle'
import AppAvatar from './AppAvatar'
import AppSubtitle from './AppSubtitle'
import AppText from './AppText'

export default {
    props: {
        blocks: Array,
        isError: Boolean
    },
    components: {AppTitle, AppAvatar, AppSubtitle, AppText}
}
</script>
