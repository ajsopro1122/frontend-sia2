<template>
    <div>
        <NavBar />
        <div class="container">
            <h1>My Mods</h1>
            <div class="row">
                <div class="col-6">
                    <h5>List of Mods</h5>
                    <hr>
                    <ul class="list-group">
                        <li class="list-group-item btn-li"  v-for="mod in mods" :key="mod.id" @click="onSelected(mod)">
                            {{mod.brand}} <span class="float-right">{{mod.value}}</span>
                        </li>
                    </ul>
                </div>
                <div class="col-4">
                    <ModView :mod="selectedMod" @saved="onChange" @newMod="onNew" @deleted="onChange" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            mods: [],
            selectedMod: {}
        }
    },
    methods: {
        async getMyMods() {
            await this.$axios.get('/api/mods')
            .then((res)=>{
                if(res.status==200) {
                    this.mods = res.data
                }
            })
        },
        onChange() {
            this.getMyMods()
            this.selectedMod = {}
        },
        onSelected(mod) {
            this.selectedMod = mod;
        },
        onNew() {
            this.selectedMod = {}
        },
    },
    created() {
        this.getMyMods()
    }
}
</script>

<style>
.row {
    padding-top: 30px;
}
.container {
    margin-top: 20px;
}
h1 {
    text-align: center;
}
.btn-li {
    cursor: pointer;
}
.btn-li:hover {
    background-color: antiquewhite;
}
</style>
