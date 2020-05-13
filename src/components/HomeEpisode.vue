<template>
  <div>
    <h1 v-if='finale' class='season-title'>Season {{season}}</h1>
    <!-- Perhaps there's a better way to do this -->
    <div class='episode-first' 
      v-if="episodeType !== 'trailer' && finale"
      @mouseover="sendScore(info)">
      <section class='player' @click='play()'>
        <audio :src='audio'></audio>
        <i class='fas fa-play-circle fa-lg' @click='play()' />
      </section>
      <aside class='ep-info'>
        <h2 class='episode-title'>
            {{title}}
            <i class="fas fa-gamepad" v-if="info"></i>
        </h2>
        <p class='ep-description'>{{description}}</p>
      </aside>
    </div>
    <div class='episode' v-else-if="episodeType !== 'trailer'"
      @mouseover="sendScore(info)">
      <section class='player' v-on:click='play()'>
        <audio :src='audio'></audio>
        <i class='fas fa-play-circle fa-lg' @click='play()' />
      </section>
      <aside class='ep-info'>
        <h2 class='episode-title'>
            {{title}}
            <i class="fas fa-gamepad" v-if="info"></i>
        </h2>
        <p class='ep-description'>{{description}}</p>
      </aside>
    </div>
  </div>
</template>

<script lang='ts'>
import { Component, Prop, Vue } from 'vue-property-decorator'
import IRankingInfo from '../interfaces/IRankingInfo';

@Component
export default class HomeEpisode extends Vue {
  @Prop() title!: string;
  @Prop() description!: string;
  @Prop() guest!: boolean;
  @Prop() audio!: string;
  @Prop() season!: number;
  @Prop() episodeNumber!: number;
  @Prop() episodeType!: string;
  @Prop() info!: IRankingInfo;
  @Prop() finale!: boolean;

  public sendScore (rankingInfo: IRankingInfo) {
    if(rankingInfo){
      this.$emit('show-score', [rankingInfo, this.title]);
    }
  }

  public play () {
    console.log('Hello World')
  }
}
</script>

<style scoped>
.season-title {
  color: red;
  width: 100%;
  border-bottom: solid red;
  margin-bottom: 0;
}
.episode,
.episode-first {
  display: grid;
  align-items: center;
  grid-template-columns: 50px auto;
  width: 100%;
  padding-top: 20px;
  padding-bottom: 20px;
  overflow: hidden;
}
.episode {
  border-top: #2d32af solid 1px;
}
.player > svg {
  font-size: 32px;
}
.player > svg:hover {
  cursor: pointer;
}
.episode-title {
  color: #2d32af;
  font-size: 1.3rem;
  margin-bottom: 0;
  margin-top: 0;
}
.episode-title > svg {
  color: red;
}
.ep-description {
  color: #2d32af;
  margin-top: 5px;
  margin-bottom: 5px;
}
.episode:hover > .ep-info > h2 > svg,
.episode-first:hover > .ep-info > h2 > svg {
  animation: rumble 0.3s ease-out;
}
@keyframes rumble {
  20% {
    transform: translateY(-3px);
  }
  40% {
    transform: translateY(3px);
  }
  60% {
    transform: translateY(-1px);
  }
  80% {
    transform: translate(1px);
  }
  100% {
    transform: translateY(0px);
  }
}

/* Small devices (portrait tablets and large phones, 600px and up) */
@media only screen and (min-width: 768px) {
  .player > svg {
    font-size: 32px;
  }
}

/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (min-width: 992px) {
  .episode:hover,
  .episode-first:hover {
    background: #f1f1f1;
  }
}
</style>