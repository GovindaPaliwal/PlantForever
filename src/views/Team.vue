<template>
  <div>
    <div class="banner-container">
      <div class="title">MEET OUR TEAM</div>
    </div>
    <div class="profile-cards">
      <div v-for="person in teamData" class="card">
        <div class="name">{{ person.name }} — {{ person.title }}</div>
        <img :src="require(`@/assets/team/${ person.photo }.jpg`)" :alt="`${ person.name } profile picture`">
        <div v-for="paragraph in person.story" class="story">
          {{ paragraph }}
          <br />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Team",
  metaInfo: {
    title: "Meet Our Team",
    meta: [
      { name: "description", content: "Meet the endeavours of PlantForever. We are a group of environmentalist who intend to inspire a movement empowering individuals to take action globally." },
      { name: "keywords", content: "PlantForever, organization, team, who, meet, founder, people, environmentalist, Marmik Patel, Ching Chang" }
    ],
    link: [
      { rel: "canonical", href: "https://www.plantforever.org/meet-our-team" }
    ]
  },
  data () {
    return {
      teamData: [
        {
          name: "Marmik Patel",
          title: "Founding President",
          photo: "marmik",
          story: ["Marmik founded PlantForever in 2017 to create change. After learning about the limited time we have until the climate crisis poses irreversible global changes, Marmik took the initiative and began planting trees. Since then, Marmik has transformed a mere idea into a fully functioning organization; he assembled a team, launched city-wide tree planting initiatives, created local partnerships, and organized platforms to spread awareness. Marmik is proud of his city’s urban forest but knows it can expand", "Now 17, Marmik aims to develop PlantForever internationally, spreading awareness and encouraging vast tree-canopy expansion in urban areas. He intends to inspire a movement empowering individuals to take action globally"]
        },
        {
          name: "Pedro Almeida",
          title: "VP of Human Resources",
          photo: "pedro",
          story: ["Pedro, now 17, is looking forward to his final year before post-secondary school where he will focus on his schooling as well as assisting the executive team to expand PlantForever. Pedro facilitates communication between members as well as with people outside of the organization. He also assists the team in managing large events"]
        },
        {
          name: "Ching Chang",
          title: "Web Developer",
          photo: "ching",
          story: ["As a 17-year old environmentalist, Ching joined PlantForever to mitigate the climate crisis. With a passion for web developing, Ching modernized the ways that people can interact with, learn from, and contribute to PlantForever"]
        },
        {
          name: "Fatima Shamas",
          title: "Social Media Manager",
          photo: "logo",
          story: ["Fatima joined PlantForever in 2018 as a volunteer, but later became a part of the team in hopes of spreading awareness through social media. By managing the PlantForever social media, her goal remains the same: to educate and inform people about climate change and help them make positive environmental changes within their lifestyle"]
        },
        {
          name: "Manraj Singh",
          title: "VP of Marketing",
          photo: "raj",
          story: ["Being only 17 year old and an environmentalist, having a place where a person can make a change was the reason he joined PlantForever. He loves working with the community and helping both PlantForever and climate change get recognition. His goal is to grow PlantForever to a national level. He is one of the voices which helps PlantForever move forward"]
        },
        {
          name: "Vanshaj Upadhyay",
          title: "Merch & Design Manager",
          photo: "vanshaj",
          story: ["Vanshaj first joined PlantForever as a volunteer. He is currently the manager of Merchandise and Design, and now manages the entire Merchandise department at Plantforever. With a passion for climate and trees, his goal is to educate and help people make a positive difference for the environment"]
        },
        {
          name: "Eric Li",
          title: "Photographer",
          photo: "logo",
          story: ["Eric initially joined PlantForever as a volunteer, helping the team with minor editing tasks. With a hobby in photography, he officially joined the PF team in mid 2019 as the official photographer. Eric now works with all the team members, especially during planting events, to capture photos"]
        }
      ]
    }
  },
  mounted() {
    this.animateCards();
    window.addEventListener("resize", this.animateCards);
    window.addEventListener("scroll", this.animateCards);
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.animateCards);
    window.removeEventListener("scroll", this.animateCards);
  },
  methods: {
    animateCards() {
      for (let index = 0; index < document.querySelectorAll(".card").length; index++) {
        if (window.innerHeight > document.querySelectorAll(".card")[index].getClientRects()[0].top) {
          document.querySelectorAll(".card")[index].classList.add("slide-up");
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.profile-cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  margin: 25px 15px 35px 15px;
  .card {
    box-shadow: 0 1px 5px 0 rgba($green, 0.5);
    margin: 15px;
    width: 650px;
    display: flex;
    flex-direction: column;
    align-items: center;
    transition-duration: 0.9s;
    opacity: 0;
    transform: translateY(100px);
    .name {
      margin: 12px;
      font-size: 25px;
      font-weight: 600;
    }
    img {
      border: 10px solid $green;
      border-radius: 50%;
      padding: 5px;
      width: 125px;
      height: 125px;
    }
    .story {
      margin: 15px;
      width: 90%;
      font-family: sans-serif;
      font-size: 17px;
    }
    &.slide-up {
      opacity: 1;
      transform: translateY(0px);
    }
  }
}
</style>