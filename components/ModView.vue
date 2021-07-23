<template>
    <div>
        <button class="btn btn-success float-right" @click="onNew">
            New Mod
        </button>
        <h5>Mod View</h5>
        <hr>

        <form action="" @submit.prevent="onSave">
            <b-form-group label="Brand">
                <b-form-input v-model="mod.brand"></b-form-input>
            </b-form-group>
            <b-form-group label="Model">
                <b-form-input v-model="mod.model"></b-form-input>
            </b-form-group>
            <b-form-group label="Description">
                <b-form-input v-model="mod.description"></b-form-input>
            </b-form-group>
            <b-form-group label="Value">
                <b-form-input v-model="mod.value"></b-form-input>
            </b-form-group>
            <b-form-group
              label="Type"
              label-for="type"
            >
              <b-form-select v-model="mod.type" :options="types"></b-form-select>
            </b-form-group>
            <b-form-group>
                <button class="btn btn-primary" type="submit">Save Changes</button>
                <button class="btn btn-danger float-right" type="button" @click="onDelete" v-if="mod.id">Delete Mod</button>
            </b-form-group>
        </form>
    </div>
</template>

<script>
export default {
    props: {
        mod: {},
    },
    data() {
      return {
        types: [
          {value: 'variable', text: 'Variable'},
          {value: 'mechanical', text: 'Mechanical'}
        ]
      }
    },
    methods: {
        async onSave() {
            try {
                if(!this.mod.id) {
                    await this.$axios.post('/api/mods', this.mod)
                }else{
                    await this.$axios.put('/api/mods/' + this.mod.id, this.mod)
                }

                this.$emit('saved')
            } catch (err) {
                alert(err.response.data.message)
            }
        },
        onNew() {
            this.$emit('newMod')
        },
        async onDelete() {
            try {
                this.$axios.delete('/api/mods/' + this.mod.id)
                this.$emit('deleted')
            } catch (err) {
                alert(err.response.data.message)
            }
        }
    }
}
</script>
