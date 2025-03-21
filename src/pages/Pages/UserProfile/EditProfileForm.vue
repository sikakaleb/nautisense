<template>
  <card>
    <h5 slot="header" class="title">Edit Profile</h5>
    <form @submit.prevent="updateProfile">
      <div class="row">
        <div class="col-md-5">
          <base-input
            type="text"
            label="Company"
            :disabled="true"
            placeholder="Company"
            v-model="user.company"
          >
          </base-input>
        </div>
        <div class="col-md-3">
          <base-input
            type="text"
            label="Username"
            placeholder="Username"
            v-model="user.username"
          >
          </base-input>
        </div>
        <div class="col-md-4">
          <base-input
            type="email"
            label="Email address"
            placeholder="mail@email.com"
            v-model="user.email"
          >
          </base-input>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6">
          <base-input
            type="text"
            label="First Name"
            placeholder="First Name"
            v-model="user.firstName"
          >
          </base-input>
        </div>
        <div class="col-md-6">
          <base-input
            type="text"
            label="Last Name"
            placeholder="Last Name"
            v-model="user.lastName"
          >
          </base-input>
        </div>
      </div>

      <div class="row">
        <div class="col-md-12">
          <base-input
            type="text"
            label="Address"
            placeholder="Home Address"
            v-model="user.address"
          >
          </base-input>
        </div>
      </div>

      <div class="row">
        <div class="col-md-4">
          <base-input
            type="text"
            label="City"
            placeholder="City"
            v-model="user.city"
          >
          </base-input>
        </div>
        <div class="col-md-4">
          <base-input
            type="text"
            label="Country"
            placeholder="Country"
            v-model="user.country"
          >
          </base-input>
        </div>
        <div class="col-md-4">
          <base-input
            label="Postal Code"
            placeholder="ZIP Code"
            v-model="user.postalCode"
          >
          </base-input>
        </div>
      </div>

      <div class="row">
        <div class="col-md-12">
          <base-input label="About Me">
            <textarea
              class="form-control"
              placeholder="ZIP Code"
              v-model="user.aboutMe"
            >
            </textarea>
          </base-input>
        </div>
      </div>

      <base-button native-type="submit" type="primary" class="btn-fill">
        Save
      </base-button>
    </form>
  </card>
</template>
<script>
export default {
  data() {
    return {
      user: {
        company: 'Nautisense',
        username: 'NautisenseCeo6385',
        email: '',
        firstName: 'Philomene',
        lastName: 'Rhodet',
        address: 'Campus Sophia Tech 930 rte Colles, 06410 Biot France',
        city: 'Biot',
        country: 'France',
        postalCode: '06410',
        aboutMe: `Exploring the crossroads of technology and creativity. Driven by passion, guided by purpose, and constantly evolving to turn ideas into impactful realities.`
      }
    };
  },
  methods: {
    updateProfile() {
      alert('Your data: ' + JSON.stringify(this.user));
    }
  }
};
</script>
<style></style>
