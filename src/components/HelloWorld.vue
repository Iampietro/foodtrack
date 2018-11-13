<template>
  <div class="container">
    <h1>FoodTruck application</h1>
    <form>

      <div class="form-group">
        <label for="sel1">How many employees ?:</label>
        <select class="form-control" id="sel1" v-model="form.employee" required>
          <option v-for="e in employees">
            {{ e }}
          </option>
        </select>
      </div>

      <div class="form-group">
        <label>At what time ?:</label>
        <select class="form-control" id="sel2" v-model="form.hour" required>
          <option v-for="h in hours">
            {{ h }}
          </option>
        </select>
      </div>


      <div>
        <h4 v-show="lambda">lambda: {{ uLambda }}</h4>
        <h4 v-show="probWaiting">Probability of waiting Ψ: {{ uProbWaiting }}</h4>
           <h4 v-show="noUnits">Probability of No Units: {{ uNoUnits }}</h4>
           <h4 v-show="noUnits">Average of Units on Queue: {{ uAvgUnits }}</h4>
           <h4 v-show="noUnits">Average of Units on System: {{ uAvgUnitsSystem }}</h4>
           <h4 v-show="noUnits">Average of Time waiting on Queue: {{ uAvgTime }}</h4>
           <h4 v-show="noUnits">Average of Time waiting on System: {{ uAvgTimeSystem }}</h4>
      </div>

  </form>
  </div>
</template>

<script>
import salesService from './service';
export default {

  data () {
    return {
      mu: 0.30,
      form: {
        hour: '',
        employee: '',
        lambda: ''
      },
      results: {
        lambda: '',
        probWaiting: '',
        probNoUnits: '',
        avgUnitsOnQueue: '',
        avgUnitsOnSystem: '',
        avgTimeWaitingOnQueue: '',
        avgTimeWaitingOnSystem: ''
      },
      employees: [
        
        '1', '2', '3'
      ],
      hours: [
        
        '18:00', '19:00', '20:00'
      ],
      sales: []
    }
  },
  computed:{
    lambda(){
      return this.form.hour != '';
    },
    uLambda(){
      return this.getLambda(this.form.hour);
    },
    updateLambda(){
      if(this.form.hour != null && this.form.hour != ''){
        this.results.lambda = this.getLambda(this.form.hour);
      }
    },
    probWaiting(){
      return this.form.hour != '' && this.form.employee != '';
    },
    uProbWaiting(){
      return salesService.getProbabilityOfWaiting(this.form.hour, this.form.employee, this.mu);
    },
    updateProbWaiting(){
      if(this.form.hour != null && this.form.hour != '' && this.form.employee != ''){
        this.results.probWaiting = salesService.getProbabilityOfWaiting(this.form.hour, this.form.employee, this.mu);
      }
    },
    noUnits(){
      return this.form.hour != '' && this.form.employee != '';
    },
    uNoUnits(){
      return salesService.getProbabilityOfNoUnits(this.form.hour, this.form.employee, this.mu)
    },
    updateProbOfNoUnits(){
      if(this.form.hour != null && this.form.hour != '' && this.form.employee != ''){
        this.results.probNoUnits = salesService.getProbabilityOfNoUnits(this.form.hour, this.form.employee, this.mu);
      }
    },
    uAvgUnits(){
      return salesService.getAverageOfUnitsOnQueue(this.form.hour, this.form.employee, this.mu);
    },
    updateAvgOfUnitsOnQueue(){
      if(this.form.hour != null && this.form.hour != '' && this.form.employee != ''){
        this.results.avgUnitsOnQueue = salesService.getAverageOfUnitsOnQueue(this.form.hour, this.form.employee, this.mu);
      }
    },
    uAvgUnitsSystem(){
      return salesService.getAverageOfUnitsOnSystem(this.form.hour, this.form.employee, this.mu);
    },
    updateAvgUnitsOnSystem(){
      if(this.form.hour != null && this.form.hour != '' && this.form.employee != ''){
        this.results.avgUnitsOnSystem = salesService.getAverageOfUnitsOnSystem(this.form.hour, this.form.employee, this.mu);
      }
    },
    uAvgTime(){
      return salesService.getAverageTimeOfWaitingOnQueue(this.form.hour, this.form.employee, this.mu);
    },
    updateAvgTimeWaitingOnQueue(){
      if(this.form.hour != null && this.form.hour != '' && this.form.employee != ''){
        this.results.avgTimeWaitingOnQueue = salesService.getAverageTimeOfWaitingOnQueue(this.form.hour, this.form.employee, this.mu);
      }
    },
    uAvgTimeSystem(){
      return salesService.getAverageTimeOfWaitingOnSystem(this.form.hour, this.form.employee, this.mu);
    },
    updateAvgTimeWaitingOnSystem(){
      if(this.form.hour != null && this.form.hour != '' && this.form.employee != ''){
        this.results.avgTimeWaitingOnSystem = salesService.getAverageTimeOfWaitingOnSystem(this.form.hour, this.form.employee, this.mu);
      }
    }
  },
  methods: {
    getLambda(hour){
      const sales = this.getSalesByHour(hour);
      let lambda = sales.length / 60; //cantidad de clientes que llegaron en esa hora (60 minutos)
      return lambda;
    },
    getSalesByHour(hour){
      let filter = [];
      this.sales.forEach(function(s) {
              if(s.hour == hour){
                  filter.push(s);
              }
          });
        return filter;
    },
  },
  mounted() {
    this.sales=[
    {
        "saleId": 1,
        "hour": "18:00"
    },
    {
        "saleId": 2,
        "hour": "19:00"
    },
    {
        "saleId": 3,
        "hour": "20:00"
    },
    {
        "saleId": 4,
        "hour": "18:00"
    },
    {
        "saleId": 5,
        "hour": "19:00"
    },
    {
        "saleId": 6,
        "hour": "20:00"
    },
    {
        "saleId": 7,
        "hour": "18:00"
    },
    {
        "saleId": 8,
        "hour": "19:00"
    },
    {
        "saleId": 9,
        "hour": "20:00"
    },
    {
        "saleId": 10,
        "hour": "18:00"
    },
    {
        "saleId": 11,
        "hour": "19:00"
    },
    {
        "saleId": 12,
        "hour": "20:00"
    },
    {
        "saleId": 13,
        "hour": "18:00"
    },
    {
        "saleId": 14,
        "hour": "19:00"
    },
    {
        "saleId": 15,
        "hour": "20:00"
    },
    {
        "saleId": 16,
        "hour": "18:00"
    },
    {
        "saleId": 17,
        "hour": "19:00"
    },
    {
        "saleId": 18,
        "hour": "20:00"
    },
        {
        "saleId": 19,
        "hour": "20:00"
    },
    {
        "saleId": 20,
        "hour": "18:00"
    },
    {
        "saleId": 21,
        "hour": "19:00"
    },
    {
        "saleId": 22,
        "hour": "20:00"
    },
    {
        "saleId": 23,
        "hour": "18:00"
    },
    {
        "saleId": 24,
        "hour": "19:00"
    },
    {
        "saleId": 25,
        "hour": "20:00"
    }
]
  }
}
</script>
