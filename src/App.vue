<template>
  <div class="app container">
    <div class="py-5 text-center">
      <div class="row justify-content-md-center">
        <div class="col">
          <p class="lead">Select trailers , drivers and items to pickup and delivery.</p>
        </div>
      </div>
      <div class="row justify-content-md-center">
    <form @submit.prevent="handleDelivery($event)" class="col-6">
      <div class="col-md-12 mb-3">
        <div class="form-group">
            <label for="state">Trailers</label>
            <select class="form-control" id="trailers" name="trailer" v-model="form.trailer" required="">
              <option >Choose...</option>
              <option v-for="(trailer , index) in trailers" :key="index" :value="trailer">{{trailer.name}}</option>
            </select>
        </div>
      </div>

       <div class="col-md-12 mb-3">
        <div class="form-group">
            <label for="state">Drivers</label>
            <select class="form-control" id="driver" name="driver" v-model="form.driver" required="">
              <option >Choose...</option>
              <option v-for="(driver , index) in drivers" :key="index" :value="driver">{{driver.name}}</option>
            </select>
        </div>
      </div>


       <div class="col-md-12 mb-3">
        <div class="form-group">
          <div class="form-check form-check-inline" v-for="(item , index) in items" :key="index">
            <input class="form-check-input"  :id="item.id" type="checkbox" :value="item.id" v-on:change="onChange(item , $event)" >
            <label class="form-check-label" for="inlineCheckbox1">{{item.name}}</label>
          </div>
        </div>
      </div>
       <div class="col-md-12 mb-3">
      <button class="btn btn-lg btn-primary btn-block" type="submit">Make a trip</button>
</div>
</form>

            <div class="valid-feedback" v-if="validTrips.trailer">
              The full Capacity for trailer <strong>{{validTrips.trailer.name}}</strong>  is <strong> {{validTrips.trailer.capacity}}</strong> and the size of  items  <strong>{{validTrips.fullSize}} </strong>
              <h6>Suggested Trip</h6>
                <table class="table table-striped">
                  <tbody>
                    <tr  v-for="(trip,  index) in validTrips.trip" :key="index">
                      <th scope="row">{{index}}</th>
                      <td> <label>From</label> :<span> {{trip.pickup}}</span></td>
                      <td> <label>To</label> :<span> {{trip.delivery}}</span></td>
                    </tr>
                  </tbody>
                </table>
            </div>

            <div class="invalid-feedback" v-for="(err , index) in errors" :key="index">
              {{err}}
            </div>

        </div>
  </div>
  </div>
</template>

<script>
import '../node_modules/bootstrap/dist/css/bootstrap.min.css';

export default {
  name: 'app',
  components: {
  },
  data() {
    return {
          trailers : [
                { id: 0,  name : 'trailer 1',  capacity:  25 , avaliable : true} , { id: 1,  name : 'trailer 2',  capacity:  30 , avaliable : false} , { id: 2,  name : 'trailer 3',  capacity:  40 ,  avaliable : false} , { id: 3,  name : 'trailer 4',  capacity:  15 , avaliable : true } ,{ id: 4,  name : 'trailer 5',  capacity:  22 , avaliable : true}
            ],
          drivers : [
            {id: 0, name :'Alex' , avaliable : true } , {id: 1, name :'Jack' , avaliable : true } , {id: 2, name :'Mick' , avaliable : true },  {id: 3, name :'Adam' , avaliable : false }
          ] ,
          items : [
            {id: 0, name :'item 1' , status:'pending' , pickup_location:'Frankfurt', delivery_location:'Hofstr' , size:10 },
            {id: 1, name :'item 2' , status:'pending' , pickup_location:'Burgunderstr', delivery_location:'Schleswigerstr'  , size:8},
            {id: 2, name :'item 3' , status:'pending' , pickup_location:'Frankfurt', delivery_location:'Ulmenweg' ,size:5 },
            {id: 3, name :'item 4' , status:'pending' , pickup_location:'Lilienthalstr', delivery_location:'Hofstr' , size:30 },
            {id: 4, name :'item 5' , status:'pending' , pickup_location:'Bremen', delivery_location:'Burgunderstr',size:12 },
          ],
          form: {
            trailer: '',
            driver: '',
            items: [],
          },
          errors:[],
          validTrips: [],
    }
  },
  methods: {
    handleDelivery(evt) {
      evt.preventDefault();
      this.errors = [];
      if(this.checkAvalibility(this.form.driver)  && this.checkAvalibility(this.form.trailer)) {
        // then handle the capacity of items and trailers
        // first check is there any items
        if(this.form.items.length > 0) {
         const fullSize =  this.handleCapacity(this.form.items);
         if(fullSize > this.form.trailer.capacity) {
           this.errors.push(`The Items size ${fullSize} is more than the trailer capacity ${this.form.trailer.capacity}` );
         } else {
           const path = this.form.items.map(item => {
             return {pickup : item.pickup_location , delivery :  item.delivery_location };
           })
           this.validTrips = {
             driver : this.form.driver,
             trailer: this.form.trailer,
             fullSize,
             trip : path
           };
           }
        }
      } else {

      }
    },
     onChange(item , e) {
       this.form.items.push(item);
      //  if(this.form.items.length > 0) {
      //     this.form.items.find(itm => {
      //       if(itm.id === item.id) {
      //               this.form.items.splice(item.id , 1)
      //       } else {
      //         this.form.items.push(item);
      //       }
      //     });
      //  } else {

      //  }
    },
    handleCapacity(items) {
      let finaclCapcity = 0;
      this.form.items.map(itm => {
        finaclCapcity += itm.size;
      });
      return finaclCapcity;
    },
    checkAvalibility(item) {
      if(!item.avaliable) {
        this.errors.push(`${item.name} is not available now !!`)
        return false;
      } else {
        return item.avaliable
      }
    }
  }
}
</script>
<style>
  .invalid-feedback , .valid-feedback {
    display: block;
  }
</style>
