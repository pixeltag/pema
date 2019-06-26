<template>
  <div class="app container">
    <div class="py-5 text-center">
      <div class="row justify-content-md-center">
        <div class="col">
          <p class="lead">Select trailers , drivers and items to pickup and delivery.</p>
        </div>
      </div>
      <div class="row justify-content-md-center">
    <form @submit:prevent="handleDelivery()" class="col-6">
      <div class="col-md-12 mb-3">
        <div class="form-group">
            <label for="state">Trailers</label>
            <select class="form-control" id="trailers" name="trailer" v-model="form.trailer" required="">
              <option >Choose...</option>
              <option v-for="(trailer , index) in trailers" :key="index">{{trailer.name}}</option>
            </select>
        </div>
      </div>

       <div class="col-md-12 mb-3">
        <div class="form-group">
            <label for="state">Drivers</label>
            <select class="form-control" id="driver" name="driver" v-model="form.driver" required="">
              <option >Choose...</option>
              <option v-for="(driver , index) in drivers" :key="index">{{driver.name}}</option>
            </select>
        </div>
      </div>


       <div class="col-md-12 mb-3">
        <div class="form-group">
          <div class="form-check form-check-inline" v-for="(item , index) in items" :key="index">
            <input class="form-check-input" type="checkbox" :value="item.id">
            <label class="form-check-label" for="inlineCheckbox1">{{item.name}}</label>
          </div>
        </div>
      </div>
       <div class="col-md-12 mb-3">
      <button class="btn btn-lg btn-primary btn-block" type="submit">Make a trip</button>
</div>
</form>



            <div class="invalid-feedback">
              Please provide a valid state.
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
                { id: 0,  name : 'trailer 1',  capacity:  50 , avaliable : true} , { id: 1,  name : 'trailer 2',  capacity:  30 , avaliable : false} , { id: 2,  name : 'trailer 3',  capacity:  50 ,  avaliable : false} , { id: 3,  name : 'trailer 4',  capacity:  60 , avaliable : true } ,{ id: 4,  name : 'trailer 5',  capacity:  50 , avaliable : true}
            ],
          drivers : [
            {id: 0, name :'Alex' , avaliable : true } , {id: 1, name :'Jack' , avaliable : true } , {id: 2, name :'Mick' , avaliable : true },  {id: 3, name :'Adam' , avaliable : false }
          ] ,
          items : [
            {id: 0, name :'item 1' , status:'pending' , pickup_location:'Frankfurt', delivery_location:'Hofstr' },
            {id: 1, name :'item 2' , status:'pending' , pickup_location:'Burgunderstr', delivery_location:'Schleswigerstr' },
            {id: 2, name :'item 3' , status:'pending' , pickup_location:'Frankfurt', delivery_location:'Ulmenweg' },
            {id: 3, name :'item 4' , status:'pending' , pickup_location:'Lilienthalstr', delivery_location:'Hofstr' },
            {id: 4, name :'item 5' , status:'pending' , pickup_location:'Bremen', delivery_location:'Burgunderstr' },
          ],
          form: {
            trailer: '',
            driver: '',
            items: '',
          }
    }
  }
}
</script>
