<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">
        Admin
      </div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong> {{ followers }}
      </div>
      <form
        class="user-profile__create-twoot"
        @submit.prevent="createTwoot"
        :class="{ '--exceeded': newTwootCharacterCount > 180 }"
      >
        <label for="newTwoot"
          ><strong>New Twoot</strong>({{ newTwootCharacterCount }}/180)</label
        >
        <textarea id="newTwoot" rows="4" v-model="newTwootContent"></textarea>
        <div class="user-profile__create-twoot-type">
          <select name="newTwootType" v-model="selectedTwootType">
            <option
              :value="option.value"
              v-for="(option, index) in twootTypes"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>
          <button>
            Twoot
          </button>
        </div>
      </form>
    </div>
    <div class="user-profile__twoots-wrapper">
      <TwootItem
        v-for="twoot in user.twoots"
        :key="twoot.id"
        :username="user.username"
        :twoot="twoot"
        @favourite="toggleFavourite"
      />
    </div>
  </div>
</template>

<script>
import TwootItem from "./TwootItem";

export default {
  name: "UserProfile",
  components: {
    TwootItem
  },
  data() {
    return {
      newTwootContent: "",
      selectedTwootType: "instant",
      twootTypes: [
        {
          name: "Draft",
          value: "draft"
        },
        {
          name: "Instant Twoot",
          value: "instant"
        }
      ],
      followers: 0,
      user: {
        id: 1,
        username: "roger28200901",
        firstName: "Yun-Hong",
        lastName: "Chen",
        email: "roger28200901@gmail.com",
        isAdmin: true,
        twoots: [
          {
            id: 1,
            content: "This is content for number 1"
          },
          {
            id: 2,
            content: "This is content for number 2"
          }
        ]
      }
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower`);
      }
    }
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
    newTwootCharacterCount() {
      return this.newTwootContent.length;
    }
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavourite(id) {
      console.log(`Favourite tweet is #${id}`);
    },
    createTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== "draft") {
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent
        });
      }
      this.newTwootContent = "";
    }
  },
  mounted() {
    this.followUser();
  }
};
</script>

<style lang="scss" scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  text-align: left;
  padding: 50px 5%;

  h1 {
    margin: 0;
  }

  .user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #dfe3e8;

    .user-proflie__admin-badge {
      margin-right: auto;
      padding: 5px;
      border-radius: 5px;
      padding: 0 10px;
      background-color: purple;
      color: white;
      font-weight: bold;
    }

    .user-profile__create-twoot {
      display: flex;
      flex-direction: column;
      padding-top: 20px;
      border-top: 1px solid #dfe3e8;

      &.--exceeded {
        color: red;
        border-color: red;
      }
    }
  }

  .user-profile__twoots-wrapper {
    width: 90%;
  }
}
</style>
