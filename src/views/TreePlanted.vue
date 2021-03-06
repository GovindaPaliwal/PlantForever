<template>
  <div class="contains-form">
    <div class="banner-container">
      <div class="title">ACCEPT A TREE</div>
    </div>
    <div v-if="thankYouMessage" id="thank-you-message">
      <div>Thank you for helping the environment!</div>
      <div>We will contact you back for updates!</div>
      <div id="dismiss-button" @click="thankYouMessage = false">OK</div>
    </div>
    <div>
      <div class="text-container">
        <div class="text">
          If you live in the Edmonton area and are looking for young trees to plant on your property,
          PlantForever is here to help!
        </div>
        <div class="text">
          Plantation of up to one tree is free, while each extra tree requires a minimum donation of $10 to allow us to keep planting.
        </div>
        <div class="text">
          Homeowners must provide potting soil
        </div>
        <div class="text">
          The trees are 2 to 4 feet in size
        </div>
        <div class="text">
          It is recommended that you visit <a href="http://albertaonecall.com" target="_blank" rel="noopener noreferrer">Call Before You Dig</a> to avoid utilities under you home and determine the areas where the plantation is possible.
        </div>
      </div>
      <form id="volunteer-form" @submit.prevent="submit">
        <div style="color: red;">* Required</div>
        <div class="row">
          <input v-model="name" type="text" name="name" placeholder="Name" required /><span class="star" style="transform: translateX(-1em);">*</span>
          <input v-model="email" type="text" name="email" placeholder="Email" required /><span class="star" style="transform: translateX(-1em);">*</span>
          <input v-model="phone" type="text" name="phone" placeholder="Phone" />
        </div>
        <div class="row">
          <input v-model="address" class="short-answer" type="text" name="address" placeholder="Address" required style="width: 96%;" />
          <span class="star">*</span>
        </div>
        <div class="text">What type(s) of tree and how many would you like? <span class="star">*</span></div>
        <div class="checkbox-container" required>
          <label><input v-model="preferredList[0]" class="checkbox" type="checkbox" /><span class="checkmark"></span>Black Cherry<span v-if="preferredList[0]"> X <input v-model="amountList[0]" class="amount" /></span></label>
          <label><input v-model="preferredList[1]" class="checkbox" type="checkbox" /><span class="checkmark"></span>Colorado Spruce<span v-if="preferredList[1]"> X <input v-model="amountList[1]" class="amount" /></span></label>
          <label><input v-model="preferredList[2]" class="checkbox" type="checkbox" /><span class="checkmark"></span>Red Maple<span v-if="preferredList[2]"> X <input v-model="amountList[2]" class="amount" /></span></label>
          <label><input v-model="preferredList[3]" class="checkbox" type="checkbox" /><span class="checkmark"></span>Schubert Chokecherry<span v-if="preferredList[3]"> X <input v-model="amountList[3]" class="amount" /></span></label>
          <input v-model="amountList" type="hidden" name="preferred_task" />
        </div>
        <div class="text">Are you able to provide any supplies?</div>
        <input v-model="materials" class="short-answer" type="text" name="materials" placeholder="e.g. shovel, gloves..." autocomplete="off" />
        <div class="text">When do you want your tree<span v-if="numberOfTrees > 1">s</span> to be planted? <span class="star">*</span></div>
        <input v-model="availability" class="short-answer" type="text" name="availability" placeholder="e.g. In the last 2 weeks of May" autocomplete="off" required />
        <div class="text">Any questions or comments?</div>
        <input v-model="comments" class="short-answer" type="text" name="comments" placeholder="Comments" autocomplete="off" />
        <button id="submit-form" class="primary-button" type="submit">Submit</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Tree",
  metaInfo: {
    title: "Accept A Tree",
    link: [
      { rel: "canonical", href: "https://www.plantforever.org/accept-a-tree/in-backyard" }
    ]
  },
  data() {
    return {
      email: "",
      name: "",
      address: "",
      phone: "",
      treeList: ["Black Cherry", "Colorado Spruce", "Red Maple", "Schubert Chokecherry"],
      preferredList: [false, false, false, false],
      amountList: [0, 0, 0, 0],
      preferredTrees: "",
      availability: "",
      materials: "",
      comments: "",
      thankYouMessage: false
    };
  },
  computed: {
    numberOfTrees() {
      let count = 0;
      for (let index in this.amountList) {
        count += parseInt(this.amountList[index]);
      }
      return count;
    }
  },
  watch: {
    preferredList() {
      for (let index in this.preferredList) {
        if (this.preferredList[index] && !this.amountList[index]) {
          this.$set(this.amountList, index, 1);
        } else if (!this.preferredList[index] && this.amountList[index]) {
          this.$set(this.amountList, index, 0);
        }
      }
    }
  },
  methods: {
    submit() {
      if (this.numberOfTrees > 0) {
        for (let treeIndex = 0; treeIndex < this.treeList.length; treeIndex++) {
          if (this.preferredList[treeIndex]) {
            this.preferredTrees += `${ this.treeList[treeIndex] }(${ this.amountList[treeIndex] })/`;
          }
        }
        this.preferredTrees = this.preferredTrees.slice(0, -1);
        axios.get("https://script.google.com/macros/s/AKfycbw-rxgZ9cs601Y0u8CxnfjCLIR-p7DisgdkMhfn0Q8-L9Q7UpU/exec", {
          params: {
            email: this.email,
            name: this.name,
            address: this.address,
            phone: this.phone,
            preferred_trees: this.preferredTrees,
            availability: this.availability,
            materials: this.materials,
            comments: this.comments
          }
        });
        this.email = this.name = this.address = this.phone = this.preferredTrees = this.availability = this.materials = this.comments = "";
        this.preferredList = [false, false, false, false];
        this.amountList = [0, 0, 0, 0];
        this.thankYouMessage = true;
      } else {
        alert("Please select at least one tree");
      }
    }
  }
}
</script>