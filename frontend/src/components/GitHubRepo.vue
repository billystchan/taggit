<template>
    <div class="card">
        <div class="card-content">
            <div class="media">
                <div class="media-left">
                    <figure class="image is-32x32">
                        <img class="is-rounded" v-lazy="ownerAvatarUrl">
                    </figure>
                </div>
                <div class="media-content">
                    <a class="has-text-weight-bold" :href="githubLink">{{ repoName }}</a>
                </div>
            </div>
            <div class="content">
                <p>{{ githubDescription }}</p>
            </div>
            <div>
                <b-field>
                    <b-taginput
                            v-model="tags"
                            ellipsis
                            icon="label"
                            placeholder="Add a tag"
                            size="is-small"
                            type="is-dark"
                            v-on:add="saveTag"
                            v-on:remove="removeTag">
                    </b-taginput>
                </b-field>
            </div>
        </div>
    </div>
</template>

<script>
  import axios from "axios";
  import { TAGGIT_BASE_API_URL } from "../common/config";

  export default {
    data() {
      return {
        tags: [],
        isTagInputVisible: false,
      }
    },
    props: ['id', 'repoName', 'githubLink', 'githubDescription', 'ownerAvatarUrl', 'metadata'],
    name: "GithubRepo",
    methods: {
      saveTag(tag) {
        axios.post(TAGGIT_BASE_API_URL + '/repo/' + this.id + '/tag',
            {
              tag
            },
            {
              'Content-Type': 'application/json'
            })
      },
      removeTag(tag) {
        axios.delete(TAGGIT_BASE_API_URL + '/repo/' + this.id + '/tag/' + tag,
            {
              'Content-Type': 'application/json'
            })
      },
      mountTags() {
        if (!(this.metadata === undefined) && !(this.metadata === null)) {
          this.tags = this.metadata.tags;
        }
      },
      makeTagInputVisible() {
        this.isTagInputVisible = !this.isTagInputVisible
      }
    },
    mounted() {
      this.mountTags()
    }
  }
</script>

<style scoped>

</style>
