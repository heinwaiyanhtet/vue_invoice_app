<template>
  <div>
    <div class="container">
      <div class="row">
        <div class="col-12">
          <h3 class="text-center my-3">Invoice Maker
            <a href="" target="_blank">
              <i class="fa-solid fa-file-pen ms-2"></i>
            </a>
          </h3>

          <form action="" class="hide-in-print" @submit.prevent="saveRecord">
            <div class="row g-2">
              <div class="col-6">
                <select class="form-select" v-model="selectedService">
                  <option value="">Select Service</option>
                  <option v-for="service in services" :value="service.id" :key="service.id">
                    {{service.name}}
                  </option>
                </select>
              </div>

              <div class="col-4">
                <input type="number" min="1" v-model.number="inputQuantity" class="form-control" placeholder="Quantity">
              </div>

              <div class="col-2">
                <button class="btn btn-primary w-100">
                  <i class="fa-solid fa-plus"></i>
                </button>
              </div>
            </div>
          </form>

          <table class="table table-bordered mt-3">
            <tr>
              <th>#</th>
              <th>Services</th>
              <th>Unit Price</th>
              <th>Quantity</th>
              <th>Cost</th>
            </tr>

            <tbody>

            <tr>
                 <td v-if="records.length === 0" colspan="5" class="text-center">No record</td>
            </tr>
            <Row v-for="record in records" @to-del="del" :key="record.id" :record="record"></Row>
            </tbody>
            <tfoot>
              <tr v-if="records.length > 0">
                <td colspan="4">Total</td>
                <td class="text-end">
                  {{ records.reduce(
                    (pV,cV)=> pV + cV.cost , 0
                )}}
                </td>
              </tr>
            </tfoot>
          </table>

           <div class="row g-2 hide-in-print">
             <div class="col-6">
               <input v-model="inVoiceNumber" class="form-control">
             </div>
             <div class="col">
               <button @click="toPrint()" class="btn btn-primary w-100">
                 <i class="fa-solid fa-print fa-fw"></i>
                 Print
               </button>
             </div>
             <div class="col">
               <button @click="save()" class="btn btn-primary w-100">
                 <i class="fa-solid fa-file fa-fw"></i>
                   save
               </button>
             </div>
           </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Row from "./components/Row";
  export default {
    components:{Row},
    data(){
      return{
        inputQuantity:"",
        selectedService:"",
        services:[
          {
            id:1,
            name : "Web Design",
            price : 100,
          },
          {
            id:2,
            name : "domain service",
            price: 10
          },
          {
            id:3,
            name: "SSL",
            price: 5
          },
          {
            id:4,
            name: "maintenance",
            price: 50
          },
        ],
        records:[],
        recordStart:1,
      }
    },
    computed:{
      inVoiceNumber()
      {
        let d = new Date();
        let dataCode = d.getDate() + "" + (d.getMonth()+1) + "" + d.getFullYear();
        let random = Math.floor(Math.random()*10000)
        return dataCode + " _ " + random;
      }
    },

    methods:{
      saveRecord()
      {
         let currentService = this.services.find(service => service.id === this.selectedService);
         let cost =   currentService.price * this.inputQuantity;

         let record = {
            id : this.recordStart,
            service : currentService,
            quantity : this.inputQuantity,
            cost
         };

         this.records.push(record);
         this.inputQuantity = this.selectedService = "";
         this.recordStart++;
      },

      del(id)
      {
        this.records = this.records.filter( record => record.id != id);
      },

      toPrint()
      {
        print();
      },

      save()
      {

      }

    }
  }
</script>

<style lang="scss">

@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@300;400;700&display=swap');
$font-family-sans-serif : 'Oswald', sans-serif;
$primary:pink;

@import "~bootstrap/scss/bootstrap";
@import "~@fortawesome/fontawesome-free/css/all.min.css";
@import "~animate.css/animate.min.css";

  @media screen {
    .show-in-print {
      display: none;
    }
  }
  @media print{
      .hide-in-print{
        display: none !important;
      }

      .show-in-print{
        display: block;
      }
  }


</style>