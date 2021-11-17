<template>
    <div>
        <div :key="data.id" v-for="data in queryData">
            <h1>name:{{data.name}}</h1>
            <h1>id:{{data.id}}</h1>
            
            <img :src="'https://image.tmdb.org/t/p/w500' + data.poster_path" alt="">
            <h1>first_air_date:{{data.first_air_date}}</h1>
            <h1>origin_country:{{data.origin_country}}</h1>
        </div>
    </div>
</template>
<script>

export default{
    name : 'DataLoader',
    props : {
        queryData : Array
    },
    
}

</script>
