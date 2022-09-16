<template>
  <div class="pt140" >
    <h1>{{ headerMessage }}</h1>

    <div class="form" v-if="!hasRegistered">
      <div class="mw380">
        <div v-if="error_message" class="error-text">
          {{ error_message}}
        </div>

    <section >
      <input type="text" id="name" minlength="2" maxlength="60" @change="checkPattern($event)" v-model="data.name" placeholder="Your name" required>
      <label for="name"><small class="error-text" v-if="errors['name']" ><span v-for="(e ,i ) in errors['name']" :key="i">{{ e }}</span></small></label>
    </section>

    <section>
      <input type="email" id="email" minlength="2" maxlength="100" v-model="data.email" @change="checkPattern($event)"
             pattern="^(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|&quot;(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*&quot;)@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])$"
             placeholder="Email" required>
      <label for="email"><small class="error-text" v-if="errors['email']" ><span v-for="(e ,i ) in errors['email']" :key="i">{{ e }}</span></small></label>
    </section>

    <section>
      <input type="tel" id="phone" v-model="data.phone" @change="checkPattern($event)"
             pattern="[\+]{0,1}380([0-9]{9})$" placeholder="Phone" required>
      <label for="phone">
        <small class="notify-text pl10 grey">+(380)XXX-XX-XX</small><br>
        <small class="error-text" v-if="errors['phone']" ><span v-for="(e ,i ) in errors['phone']" :key="i">{{ e }}</span></small></label>
    </section>
    <div></div>

    <div id="radio_box" v-if="positions.length > 0" >
        <p >Select your position</p>
        <section class="radio-item" v-for="(position , index)  in positions" :key="index">
          <input type="radio" name="position_id" v-model="data.position_id" :value="position.id" :id="`position${position.id}`" @change="setPosition($event)"   required >
          <label :for="`position${position.id}`">{{ position.name}}</label>
        </section>
    </div>
    <div v-else>
      <div class="loader">
      </div>
    </div>

    <div id="upload_image">
      <label class="file">
        <input type="file" id="file" aria-label="File browser example" :value="data.photo" @change="checkPhoto" required>
        <span class="file-custom"></span>
      </label>
    </div>
    <div class="mb50" >
      <small v-if="errors['photo']" class="error-text" :style="`color: ${image_error_color}`"  ><span v-for="(e ,i ) in errors['photo']" :key="i">{{ e }}</span></small>
    </div>

    <div class="mb50">
      <button class="y_btn" :class="{'disabled_btn' : !hasValid }" @click="sendForm">Sign up</button>
    </div>
    </div>
    </div>
    <div v-else>
      <img src="../assets/success-image.svg" alt="Success image">
    </div>
  </div>
</template>

<script>
export default {
  name: "FormComponent",
  beforeCreate() {
    fetch(`https://frontend-test-assignment-api.abz.agency/api/v1/positions`).then(
        res => { return res.json()})
    .then( data => {this.positions = data.positions})
    .catch(e => { console.log(e)});
  },

  data() {
    return {
      positions: [],
      token:null,
      data:{
        name: "",
        email: "",
        position_id:1,
        phone: null,
        photo:null
      },
      error_message: null,
      errors:[],
      headerMessage: 'Working with POST request',
      hasValid: false,
      hasRegistered: false,
      image_error_color: "red",
      formData : new FormData()
    }
  },

  async mounted(){
    this.formData.append('position_id' , this.data.position_id);
    await fetch('https://frontend-test-assignment-api.abz.agency/api/v1/token')
        .then(res => {return res.json()})
        .then(data => {
              if(data.success === true){
                this.token = data.token;
              }
        })
  },
  methods:{

    validate(){
      var BreakException = {};

      try {
        Object.entries(this.data).forEach(([key]) => {
          if(!this.formData.has(key)) throw BreakException;
          else this.hasValid = true;
        })
      } catch (e) {
        if (e !== BreakException) throw e;
        else this.hasValid = false;
      }
    },


    setPosition(data){
      this.setFormData('position_id' , data.target.value);
    },

    setFormData(name , value){
      if(this.formData.has(name)){
        this.formData.set(name ,value )
      }else {
        this.formData.append(name, value)
      }
      this.validate();
    },

    deleteFormData(name ){
      if(this.formData.has(name)) {
        this.formData.delete(name)
      }
      this.validate();
    },


    checkPattern(data){
      if(!data.target.validity.valid){
        data.target.style.borderColor = 'red';
        this.errors[data.target.id] = [ data.target.id.charAt(0).toUpperCase() + data.target.id.slice(1) + ' is invalid'];
        this.deleteFormData(data.target.id)
      }else{
        data.target.style.borderColor = '#D0CFCF';
        delete this.errors[data.target.id]
        this.setFormData(data.target.id , data.target.value);
      }
    },

    showImageError(message , data = false){
      if(data){
        this.image_error_color = 'green';
        this.validate();
      }else {
        this.image_error_color = 'red';
        this.deleteFormData('photo');
      }
      this.errors['photo'] = [ message ];
    },


    async checkPhoto(data){
        const fileUpload =  data.target;

        var regex = new RegExp("([a-zA-Z0-9])+(.jpeg|.jpg)$");
        if (regex.test(fileUpload.value.toLowerCase())) {

          if (typeof (fileUpload.files) != "undefined") {
            var reader = new FileReader();
            reader.readAsDataURL(fileUpload.files[0]);
            const photo = fileUpload.files[0];
            const result = await new Promise((resolve , reject) => {
                  reader.onload = function (e) {
                    var image = new Image();
                    image.src = e.target.result;

                    image.onload = function () {
                      var height = this.height;
                      var width = this.width;
                      if (height > 70 || width > 70) {
                        reject( {
                          message: "Photo size will be more  70x70",
                          data : null
                        })
                      }
                    };
                    resolve( {
                      message: "Image uploaded",
                      data : true
                    })
                  }
                });

              if(result.data ) {
                this.showImageError(result.message , true);
                this.setFormData('photo', photo );
              }
              else this.showImageError(result.message)
            } else this.showImageError("Something was wrong");
        } else this.showImageError( "Please select a valid Image file." );
    },

    async sendForm(){
      if(this.hasValid) {
        const headers = {
          'Token': this.token
        }
        await fetch(`https://frontend-test-assignment-api.abz.agency/api/v1/users`, {
            method: 'POST',
            headers: headers,
            body: this.formData
          }).then( res => {return res.json();})
          .then( data => {
            this.errors = [];
            if (data.success) {
              this.error_message = false;
              this.headerMessage = data.message;
              this.hasRegistered = true;
            }else if(data.fails){
              this.error_message = false;
              this.image_error_color = 'red'
              this.errors = data.fails
            }else if(data.message){
              this.error_message = data.message;
              console.log("Ops : something was wrong ");
            }
          }).catch(e => { console.log(e)});
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.form {
  display: flex;
  align-items: center;
  justify-content: center;
  .mw380{
    max-width: 380px;
    min-width: 300px;
    width: 100%;
    section {
      margin: 50px 0;
      .notify-text{
        float: left;
      }
      input[type="text"], input[type="tel"], input[type="email"] {
        outline-style: none;
        cursor: pointer;
        height: 54px;
        width: 100%;
        border-radius: 4px;
        padding-left: 10px;
        background-color: #f8f8f8;
        border: 1px solid #D0CFCF;
        &::placeholder {
          font-size: 20px;
        }
        &:focus {
          border: 1px solid #D0CFCF;
        }
      }
    }
    #radio_box {
      text-align: left;
      font-size: 14px;
      section {
        margin: 0;
         input[type="radio"] {
           display: none;
           & + *::before {
             content: "";
             display: inline-block;
             vertical-align: bottom;
             width: 17px;
             height: 17px;
             margin-right: 0.9rem;
             border-radius: 50%;
             border-style: solid;
             border-width: 1px;
             border-color: #c2c0c0;
           }
           &:checked + *::before {
             background: radial-gradient(#00bdd3 0%, #00bdd3 45%, transparent 45%, transparent);
             border-color: #00bdd3;
           }
           & + * {
             display: inline-block;
             padding: 0.3rem 0;
           }
         }
      }
    }
    #upload_image {
      margin: 50px 0 35px;
      .file {
        position: relative;
        display: inline-block;
        cursor: pointer;
        width: 100%;
        padding: 2px;
        input {
          min-width: 300px;
          margin: 0;
          filter: alpha(opacity=0);
          opacity: 0;
        }
        .file-custom {
          position: absolute;
          top: 0;
          right: 0;
          left: 0;
          z-index: 5;
          height: 58px;
          background-color: #f8f8f8;
          border: 1px solid #D0CFCF;
          border-radius: .25rem;
          box-shadow: inset 0 .2rem .4rem rgba(0, 0, 0, .05);
          user-select: none;
          &:before {
            color: #b4b4b4;
            top: 4px;
            position: absolute;
            left: 100px;
            padding: 1.1rem 1rem;
            content: "Upload your photo";
          }
          &:after {
              position: absolute;
              top: 0;
              z-index: 6;
              display: block;
              content: "Upload";
              padding: 1rem 1rem;
              line-height: 1.5;
              background-color: #eee;
              border: 1px solid #0d0d0d;
              border-radius:  4px 0  0 4px;
            }
        }
      }
    }
    .disabled_btn{
      background-color: #a1a1a1;
      color: #f8f8f8;
      filter: contrast(100%)
    }
  }
}
</style>