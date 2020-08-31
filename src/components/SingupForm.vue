<template>
    <form id="signup-form" v-on: submit.prevent="submit">
        <div class="row">
            <div class="col-12 form-group">
                <label class="col-form-label col-form-label-lg">Username<span class="text-danger"></span></label>
                <input type="text" v-model.trim="$v.fullname.$model" :class="{'is-invalid': validationStatus($v.fullname)}" class="form-control form-control-lg">
                <div v-if="!$v.fullname.required" class="invalid-feedback">The Username field is required!</div>
            </div>

            <div class="col-12 form-group">
                <label class="col-form-label col-form-label-lg">Email<span class="text-danger"></span></label>
                <input type="email" v-model.trim="$v.email.$model" :class="{'is-invalid': validationStatus($v.email)}" class="form-control form-control-lg">
                <div v-if="!$v.email.required" class="invalid-feedback">The Email field is required!</div>
                <div v-if="!$v.email.email" class="invalid-feedback">The Email is not valid</div>
            </div>

            <div class="col-12 form-group">
                <label class="col-form-label col-form-label-lg">Phone<span class="text-danger"></span></label>
                <input type="text" v-model.trim="$v.phone.$model" :class="{'is-invalid': validationStatus($v.fullname)}" class="form-control form-control-lg">
                <div v-if="!$v.phone.required" class="invalid-feedback">The Phone field is required!</div>
            </div>

            <div class="col-12 form-group">
                <label class="col-form-label col-form-label-lg">Gender<span class="text-danger"></span></label>
                <select v-model.trim="$v.gender.$model" :class="{'is-invalid': validationStatus($v.gender)}" class="form-control form-control-lg">
                    <option value=""></option>
                    <option value="">Men</option>
                    <option value="">Women</option>
                    <option value="">Not Define</option>
                </select>
                <div v-if="!$v.gender.required" class="invalid-feedback">The Gender option is required!</div>
            </div>

            <div class="col-12 form-group">
                <label class="col-form-label col-form-label-lg">Country<span class="text-danger"></span></label>
                <select v-model.trim="$v.country.$model" :class="{'is-invalid': validationStatus($v.country)}" class="form-control form-control-lg">
                    <option value="">Select Country</option>
                    <option :value="c.iso" :key="c.iso" v-for="c in countryList"> {{ c.country }} </option>
                </select>
                <div v-if="!$v.country.required" class="invalid-feedback">The Country field is required!</div>
            </div>

            <div class="col-12 form-group">
                <label class="col-form-label col-form-label-lg">Career<span class="text-danger"></span></label>
                <select v-model.trim="$v.career.$model" :class="{'is-invalid': validationStatus($v.career)}" class="form-control form-control-lg">
                    <option value=""></option>
                    <option value="">Lic. en Ing. de Redes y Datos con énfasis en Sis. Inalámbricos</option>
                    <option value="">Lic. en Ingeniería en Sistemas Computacionales</option>
                    <option value="">Lic. en Sis. Comp. con énfasis en Desarrollo de Sis. Avanzados de Redes y Software</option>
                    <option value="">Lic. en Ingeniería Electrónica y de Comunicaciones</option>
                    <option value="">Lic. en Ingeniería Industrial y de Sistemas</option>
                </select>
                <div v-if="!$v.career.required" class="invalid-feedback">The Career option is required!</div>
            </div>

            <div class="col-12 form-group">
                <label class="col-form-label col-form-label-lg">Password<span class="text-danger"></span></label>
                <input type="password" v-model.trim="$v.password.$model" :class="{'is-invalid': validationStatus($v.password)}" class="form-control form-control-lg">
                <div v-if="!$v.password.required" class="invalid-feedback">The Password field is required!</div>
                <div v-if="!$v.password.minLength" class="invalid-feedback">You must have at least {{ $v.password.$params.minLength.min }} letters</div>
                <div v-if="!$v.password.maxLength" class="invalid-feedback">You must not have greater then {{ $v.password.$params.maxLength.min }} </div>
            </div>

            <div class="col-12 form-group text-center">
                <button class="btn btn-vue btn-lg col-4">Sing Up</button>
            </div>
            
        </div>
    </form>
</template>
<script>
import { required, email, minLength, maxLength } from 'vuelidate/lib/validators'
export default{
    name: 'SignupForm',
    data: function(){
        return {
            fullname: '',
            email: '',
            country: '',
            password: '',
            phone: '',
            gender: '',
            career: '',
            countryList: []
        }
    },
    validations: {
        fullname: {required},
        email: {required, email},
        gender: {required},
        phone: {required},
        career: {required},
        country: {required},
        password: {required, minLength: minLength(6), maxLength: maxLength(18)}

    },

    mounted: function(){
        var v = this;
        v.$http.get('http://localhost:4600/countries')
        .then(function(resp){
            v.countryList = resp.data;
        })
        .catch(function(err){
            console.log(err)
        })
    },

    methods: {

        resetData: function(){
            this.fullname = '';
            this.email = '';
            this.country = '';
            this.phone = '';
            this.gender = '';
            this.career = '';
            this.password = '';
        },

        validationStatus: function(validation){
            return typeof validation != "undefined" ? validation.$error : false;
        },
        submit: function(){
            this.$v.$touch();
            if(this.$v.$pendding || this.$v.$error) return;

            alert('Data Submit');
            this.$v.$reset();
            this.resetData();
        }
    }
}
</script>
<style>
.btn-vue{
    background: #97bdf0;
    color: #31485D;
    font-weight: bold;
}
</style>