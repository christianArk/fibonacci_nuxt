<template>
  <div class="container">
    <div class="card card1">
        <h1 class="title">Fibonacci Validator</h1>
        <p class="description">This application validates if a given number exists within a Fibonacci sequence range and can also list the numbers generated</p>

        <div class="card-body">
				<div class="card-text">
					<div class="row">
						<div class="col-md-12">
							<div class="form-group">
								<label>Enter Maximum Number <span class="text-danger">*</span></label>
								<input type="number" v-model="numbers.end" class="form-control">
							</div>
						</div>
					</div>
					<div class="row">
						<div class="col-md-8">
							<div class="form-group">
								<label>Enter Random Number</label>
								<input type="number" v-model="numbers.random" class="form-control">
							</div>
						</div>
						<div class="col-md-4">
							<div class="form-group">
								<label>&nbsp;</label>
								<button class="btn btn-info form-control" @click="computeResult('validate')">Validate</button>
							</div>
						</div>
					</div>
					<template v-if="alert">
						<div v-if="valid" class="row">
							<div class="col-md-12">
								<div class="alert alert-success">
									{{ numbers.message }}
								</div>
							</div>
						</div>
						<div v-if="!valid" class="row">
							<div class="col-md-12">
								<div class="alert alert-danger">
									{{ numbers.message }}
								</div>
							</div>
						</div>
					</template>
					<div class="row">
						<div class="col-md-12">
							<div class="form-group">
								<button class="btn btn-success" @click="computeResult('compute')">Submit</button>
                <button class="btn btn-danger" @click="resetForm">Reset</button>
							</div>
						</div>
					</div>
				</div>
			</div>
    </div>

    <div v-if="showResult" class="card card2 animated fadeInRight">
      <h1 class="title">Result</h1>
      <div class="card-body">
        <div class="card-text">
          <div class="row">
						<div class="col-md-12">
							<div class="text-left">
								<span class="btn btn-info result-badge" v-for='(number, index) in numbers.result' :key="index">{{ number}}</span>
							</div>
						</div>
					</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },
  data() {
			return {
				valid: false,
				alert: false,
				showResult: false,
				numbers: {
					random: 0,
					end: 0,
          result: [],
          message: ''
				}
			}
		},
		methods:{
			computeResult(type) {
        // validate
        if (this.numbers.end == 0)
        {
          this.alert = true;
          this.valid = false;
          this.numbers.message = 'Please enter a valid maximum number!'
          return
        }
				let init = 0;
				let current = 1;
				let next = 0;
				let end = this.numbers.end;
				this.numbers.result = [];
				for (var i = 0; i < end; i++) {
					this.numbers.result.push(init);
					next  = init + current;
					init = current;
					current = next;
				}
				if(type == 'validate') {
					this.showResult = false;
					if(this.numbers.result.includes(parseInt(this.numbers.random, 10))) {
						this.alert = true;
            this.valid = true;
            this.numbers.message = 'The number entered is valid!'
					} else {
						this.alert = true;
            this.valid = false;
            this.numbers.message = 'The number entered is invalid!'
					}
				} else {
					this.showResult = true;
				}
      },
      
      resetForm() {
        this.numbers = {
          end: 0,
          random: 0,
          result: []
        };
        this.showResult = false;
        this.alert = false;
      }
    },
    mounted() {
      console.log(this.$store.state.store.number)
    },
}
</script>

<style>
body {
  background: #D18B49;
  color: #D18B49;
  font-size: 14px;
  font-family: arial, sans-serif;
  line-height: 1.6em;
}

.card {
  background: #fff;
  max-width: 700px;
  /* min-height: $max-height; */
  margin: 100px auto;
  border-radius: 12px;
  box-sizing: border-box;
  padding: 48px;
  text-align: center;
  -webkit-box-shadow: 0px 0px 9px 0px rgba(51,51,51,0.64);
  -moz-box-shadow: 0px 0px 9px 0px rgba(51,51,51,0.64);
  box-shadow: 0px 0px 9px 0px rgba(51,51,51,0.64);
}

.card1 {
  margin-bottom: 10px !important;
}

.card2 {
  margin-top: 0px !important;
}

.title {
  margin: 0 0 12;
  color: #D18B49;
  font-size: 24px;
}

.description {
  margin: 0 0 12px;
  color: #333 !important;
}

.result-badge {
  font-size: 14px;
  margin-right: 5px;
  margin-bottom: 5px;
}
</style>
