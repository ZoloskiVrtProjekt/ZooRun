<template>
    <div class="container">

        <div class="nazad">
            <button class="btn btn-danger" @click="$router.go(-1)">Nazad</button>
        </div>

        <div v-if="error" class="error">
            <h3>{{error}}</h3>
        </div>

        <form>
            <div class="row">
                <div class="col">
                    <input v-model="zaposlenik.ime" type="text" class="form-control" placeholder="Ime">
                    <input v-model="zaposlenik.prezime" type="text" class="form-control" placeholder="Prezime">
                    <input v-model="zaposlenik.email" type="text" class="form-control" placeholder="Email" disabled>
                </div>
                <div class="col">
                    <input v-model="zaposlenik.brojMobitela" type="text" class="form-control" placeholder="Broj mobitela">
                    <input v-model="zaposlenik.adresa" type="text" class="form-control" placeholder="Adresa">
                </div>              
            </div>
            <button @click.prevent="upisiPodatke" class="btn btn-primary my-1">Spremi</button>
        </form>
    </div>
</template>


<script>
import db from '@/firebase/init'

export default {
    data() {
        return {
            zaposlenik:{},
            error:''
        }
    },
    created() {
        this.dohvatiZaposlenika()
            
     
    },
    methods: {
        dohvatiZaposlenika(){
            let ref = db.collection('zaposlenici').doc(this.$route.params.id)
            ref.get().then((doc) =>{
                this.zaposlenik = doc.data()
            });
        },
        podatciUneseni(){
            for(var key in this.zaposlenik){
                if(!this.zaposlenik[key]){ 
                    return true
                }
            }
            return false
        },
        upisiPodatke(){
            if(this.podatciUneseni()){
                this.error= 'Sva polja moraju biti popunjena'
            }
            else{
            db.collection('zaposlenici').doc(this.$route.params.id).update(this.zaposlenik)
            .then(() =>{
                        this.$router.go(-1)
                        
                    })
            }        
        }
    },
}
</script>

<style lang="css" scoped>
    .container{
        padding-top: 15px;
        padding-bottom: 15px;    
  }
    .nazad{
        text-align:right
    }

    .error{
        color:red;
        text-align: center;
    }

    .success{
        color:rgb(6, 209, 6);
        text-align: center;
    }

    .btn{
    margin-bottom: 10px;
  }
</style>