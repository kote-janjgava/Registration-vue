<template>      
  <q-page class="row items-center justify-evenly">
    <q-card dark bordered class="bg-white my-card col padding">
      <form>
        <q-card-section>
          <div class="text-h6 absolute-center text-black">რეგისტრაცია</div>
        </q-card-section>
        <div class=" q-mb-md">
          <q-card-section class="q-gutter-y-md column">
            <div>
              <q-input
                class="col"
                outlined
                dense
                v-model="formData.name"
                label="სახელი"
                lazy-rules
                :rules="[val => (val && val.length > 0) || 'ჩაწერეთ სახელი']"
              />
              <q-tooltip content-class="bg-red-14"> სახელი</q-tooltip>
            </div>

            <div>
              <q-input
                class="col"
                outlined
                dense
                v-model="formData.lastname"
                label="გვარი"
                lazy-rules
                :rules="[val => (val && val.length > 0) || 'ჩაწერეთ გვარი']"
              />
              <q-tooltip content-class="bg-red-14"> გვარი</q-tooltip>
            </div>
            <div>
              <q-input
                outlined
                dense
                v-model="formData.privatenumber"
                label="პირადი ნომერი"
                mask="###########"
                lazy-rules
                :rules="[
                  val => (val && val.length > 10) || 'ჩაწერეთ პირადი ნომერი'
                ]"
              />
              <q-tooltip content-class="bg-red-14"> პირადი ნომერი</q-tooltip>
            </div>

            <div>
              <q-input
                v-model="formData.dateofbirth"
                outlined
                dense
                filled
                type="date"
                hint="დაბადების თარიღი"
                lazy-rules
                :rules="[
                  val => (val && val.length > 0) || 'ჩაწერეთ დაბადების თარიღი'
                ]"
              />
              <q-tooltip content-class="bg-red-14"> დაბადების თარიღი</q-tooltip>
            </div>
            <div class="text-black"></div>

            <q-field outlined label="სქესი" dense stack-label>
              <template v-slot:control>
                <div
                  class="self-center full-width no-outline text-black"
                  tabindex="0"
                >
                  <q-radio
                    name="shape"
                    v-model="formData.gender"
                    color="blue"
                    val="მამრობითი"
                    label="მამრობითი"
                  />
                  <q-radio
                    name="shape"
                    v-model="formData.gender"
                    color="pink-13"
                    val="მდედრობითი"
                    label="მდედრობითი"
                  />
                  <q-tooltip content-class="bg-red-14"> სქესი</q-tooltip>
                </div>
              </template>
            </q-field>
            <div>
              <q-btn label="რეგისტრაცია" push @click="requestRegistration" color="primary" />
           
            </div>
          </q-card-section>
        </div>
      </form>
    </q-card>
  </q-page>
</template>

<script lang="ts">
export default {
  name: "PageIndex",

  data() {
    return {
      formData: {
        name: "",
        lastname: "",
        dateofbirth: "",
        privatenumber: "",
        gender: ""
      }
    };
  },
   methods: {
    requestRegistration: function () {
      this.$store
        .dispatch("registerApiCall", {
          name: this.name,
          lastname: this.lastname,
          privatenumber: this.privatenumber,
          dateofbirth: this.dateofbirth,
          gender: this.gender,
        })
        .then(() => {
          this.$router.push({
            name: "dashboard",
          });
        });
    },
  },
};
</script>
<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 300px
  height:50%
</style>