<template>
  <!-- <div class="agendaBlock"> -->
  <div class="agendaBlock-container active">
    <div class="agendaBlock__close">
      <img
        src="~@/assets/images/home/home-icon-close.svg"
        @click="
          () => {
            removeFixed();
            $router.push('/agenda');
          }
        "
      />
    </div>
    <div class="agendaBlock__title">
      <h1>{{info.zh.title}}</h1>
      <div class="agendaBlock__title-sub">
        <span v-for="tag in info.tags">{{tag}}</span>
      </div>
    </div>
    <div class="agendaBlock__content">
      <div class="agendaBlock__content-left">
        <div class="agendaBlock__content-left-container">
          <article class="--top">
            <p>{{description}}</p>
          </article>
          <article v-for="speaker in info.speakers">
            <h2>{{getSpeaker(speaker).zh.name}}</h2>
            <p>{{getSpeaker(speaker).zh.bio}}
            </p>
          </article>
        </div>
      </div>
      <div class="agendaBlock__content-right">
        <div class="agendaBlock__content-right-container">
          <article>
            <h2>目標聽眾</h2>
            <p>
              {{audience}}
            </p>
          </article>
          <article>
            <h2>先備知識</h2>
            <p>
              {{knowlege}}
            </p>
          </article>
          <!--
          <section class="agendaBlock_record">
            <iframe src="" frameborder="0"></iframe>
            <div class="agendaBlock-buttonsBox">
              <a href="">簡報連結</a>
              <a href="">線上 QA</a>
            </div>
            </section>
          <section v-for="speaker in info.speakers">
            <img :src="getSpeaker(speaker).avatar" alt="null" />
          </section>
          -->
        </div>
      </div>
    </div>
    <!-- </div> -->
  </div>
</template>
<script lang="ts">
import { Watch, Component, Prop, Vue } from 'vue-property-decorator';
import { Route } from 'vue-router';

import sessionData from '@/../public/json/session.json';

import head from '../util/head';


@Component({})
export default class AgendaBlock extends Vue {
  @Prop() private id!: string;
  private sessions: any = sessionData.sessions;
  private speakers: any = sessionData.speakers;
  private info: any;
  private description: string = '';
  private knowlege: string = '';
  private audience: string = '';
  public getSpeaker (id: string): any {
    return this.speakers.find((speaker: any) => (speaker.id === id));
  }
  public getSession (id: string): any {
    return this.sessions.find((session: any) => (session.id === id));
  }

  // private info () {
  //   return this.getSession(this.id);
  // }
  private created () {
    this.info = this.getSession(this.id);
    head.title(this.info.zh.title);
    head.ogTitle(this.info.zh.title);
    head.ogDescription(this.info.zh.description);
    /********************/
    const {description} = this.info.zh;
    const temp = description.split('## 目標聽眾');
    this.description = temp[0];
    const tempp = temp[1].split('## 先備知識');
    this.audience = tempp[0];
    this.knowlege = tempp[1];
  }
  private removeFixed () {
    document.body.style.overflowY = 'scroll';
  }
}
</script>
<style lang="scss">
@import '@/assets/scss/agendaBlock';
</style>
