<template>
  <div class="ht100">
    <div class="main">
      <div class="panel">
        <div class="header">
          <div class="avatar">
            <img class="img" v-bind:src="avatar" v-on:click="showProfile"/>
          </div>
          <div class="info">
            <h3 class="nickname">
              <span class="display_name">不觉细雨</span>
              <a class="opt" href="javascript:;" v-on:click="toggleSystemMenu">
                <i class="web_wechat_add"></i>
              </a>
            </h3>
          </div>
        </div>
        <searchBar></searchBar>
        <tabBar v-on:scl="showChat" v-on:srl="showRead" v-on:sctl="showContact" v-bind:showChats="showChats" v-bind:showReads="showReads" v-bind:showContacts="showContacts"></tabBar>
        <div class="nav_view">
          <chatList v-if="showChats"></chatList>
          <readList v-if="showReads"></readList>
          <contactItem v-if="showContacts"></contactItem>
        </div>

      </div>
      <contentView></contentView>
    </div>
    <div v-if="showChatroom">
      <createChatroom v-on:hcr="hiddenChatroom"></createChatroom>
    </div>
    <div v-if="showFeedback">
      <feedback v-on:hfb="hiddenFeedback"></feedback>
    </div>
    <div v-if="showProfiles" v-bind:style="{ height: '100%' }">
      <div class="overlay_mask" v-on:click="showProfiles=false"></div>
      <div id="mmpop_profile" class="mmpop profile_mini_wrap" tabindex="-1"  v-bind:style="{ top: '69px', left: '41px' }">
        <div class="profile_mini">
          <div class="profile_mini_hd">
            <div class="avatar">
              <img class="img" v-bind:src="contact.avatar" />
            </div>
          </div>
          <div class="profile_mini_bd">
            <div class="nickname_area">
              <h4 class="nickname">{{ contact.username }}</h4>
              <i v-bind:class="{web_wechat_women: contact.sex == 'female', web_wechat_men: contact.sex == 'male'}"></i>
            </div>
            <div class="meta_area">
              <div class="meta_item">
                <label class="label">个性签名：</label>
                <p class="value" contenteditable="true">{{ contact.signature }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showSystemMenu" v-bind:style="{ height: '100%' }">
      <div class="overlay_mask" v-on:click="showSystemMenu=false"></div>
      <div id="mmpop_system_menu" class="mmpop system_menu" tabindex="-1">
        <ul class="dropdown_menu">
          <li>
            <a tabindex="-1" href="javascript:;" title="发起聊天" v-on:click="createChatroom">
              <i class="menuicon_chat"></i>发起聊天
            </a>
          </li>
          <li>
            <a tabindex="-1" href="javascript:;" title="关闭桌面通知">
              <i class="menuicon_push_on"></i>关闭桌面通知
            </a>
          </li>
          <li>
            <a tabindex="-1" href="javascript:;" title="关闭声音">
              <i class="menuicon_volume_on"></i>关闭声音
            </a>
          </li>
          <li>
            <a tabindex="-1" href="javascript:;" title="意见反馈" v-on:click="feedback">
              <i class="menuicon_feedback"></i>意见反馈
            </a>
          </li>
          <li class="last_child">
            <a tabindex="-1" href="javascript:;" title="退出">
              <i class="menuicon_quit"></i>退出
            </a>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  import feedback from './feedback.vue'
  import searchBar from './SearchBar.vue'
  import tabBar from './TabBar.vue'
  import chatList from './ChatList.vue'
  import readList from './ReadList.vue'
  import contactItem from './ContactItem.vue'
  import contentView from './ContentView.vue'
  import createChatroom from './CreateChatroom.vue'

  export default {
    components: {
      feedback,
      searchBar,
      tabBar,
      chatList,
      readList,
      contactItem,
      contentView,
      createChatroom
    },
    data() {
      return {
        avatar: require('../images/user/bu.jpg'),
        showSystemMenu: false,
        showChatroom: false,
        showFeedback: false,
        showChats: true,
        showReads: false,
        showContacts: false,
        showProfiles: false
      }
    },
    computed: {
      contact() {
        return this.$store.state.contact
      }
    },
    methods: {
      toggleSystemMenu() {
        this.showSystemMenu = !this.showSystemMenu
      },
      createChatroom() {
        this.showSystemMenu = false
        this.showChatroom = true
      },
      feedback() {
        this.showSystemMenu = false
        this.showFeedback = true
      },
      hiddenFeedback() {
        this.showFeedback = false
      },
      hiddenChatroom() {
        this.showChatroom = false
      },
      showChat() {
        this.showChats = true
        this.showReads = false
        this.showContacts = false
      },
      showRead() {
        this.showChats = false
        this.showReads = true
        this.showContacts = false
      },
      showContact() {
        this.showChats = false
        this.showReads = false
        this.showContacts = true
      },
      showProfile() {
        this.showProfiles = !this.showProfiles
      }
    }
  }
</script>

<style>
  .main {
    height: 100%;
    margin: 0 auto;
  }

  .panel {
    position: relative;
    width: 280px;
    height: 100%;
    float: left;
    background: #2e3238;
  }

  .header {
    padding: 18px;
    position: relative;
  }

  .header .avatar {
    display: table-cell;
    vertical-align: middle;
    word-wrap: break-word;
    word-break: break-all;
    white-space: nowrap;
    padding-right: 10.625px;
  }

  .header .avatar .img {
    width: 40px;
    height: 40px;
    border-radius: 2px;
    display: block;
    cursor: pointer;
  }

  .header .info {
    display: table-cell;
    vertical-align: middle;
    word-wrap: break-word;
    word-break: break-all;
    width: 2000px;
  }

  .header .info .nickname .display_name {
    display: inline-block;
    font-weight: 400;
    width: 156px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    word-wrap: normal;
    color: #fff;
    font-size: 18px;
    vertical-align: top;
    line-height: 31px;
    text-decoration: none;
  }

  .header .info .nickname .opt {
    display: inline-block;
    vertical-align: top;
  }

  .web_wechat_add {
    background: url(../images/sprite31e225.png) 0 -406px;
    width: 30px;
    height: 30px;
    vertical-align: middle;
    display: inline-block;
  }

  .mmpop {
    position: absolute;
    background-color: #fff;
    z-index: 99;
    top: 0;
    left: 0;
    outline: 0;
  }

  .system_menu {
    background: transparent!important;
    z-index: 102!important;
    width: 180px;
    top: 70px;
    left: 85px;
  }

  .dropdown_menu {
    background-color: #fff;
    border-radius: 4px;
    overflow: hidden;
    border: 1px solid #d6d6d6;
    box-shadow: rgba(0,0,0,.1) 2px 3px 10px;
    min-width: 125px;
  }

  ul ol {
    padding-left: 0;
    list-style-type: none;
  }

  .dropdown_menu li a {
    display: block;
    text-decoration: none;
    color: #000;
    font-size: 14px;
    padding: 8px;
    border-bottom: 1px solid #f1f1f1;
    text-align: left;
  }

  .dropdown_menu li a:hover {
    background-color: #f5f5f5
  }

  .dropdown_menu li i {
    margin-right: 8px;
  }

  .menuicon_chat {
    background: url(../images/sprite31e225.png) 0 -66px;
    width: 34px;
    height: 34px;
    vertical-align: middle;
    display: inline-block;
  }

  .menuicon_push_on {
    background: url(../images/sprite31e225.png) 0 -236px;
    width: 34px;
    height: 34px;
    vertical-align: middle;
    display: inline-block;
  }

  .menuicon_volume_on {
    background: url(../images/sprite31e225.png) 0 -338px;
    width: 34px;
    height: 34px;
    vertical-align: middle;
    display: inline-block;
  }

  .menuicon_feedback {
    background: url(../images/sprite31e225.png) 0 -134px;
    width: 34px;
    height: 34px;
    vertical-align: middle;
    display: inline-block;
  }

  .menuicon_quit {
    background: url(../images/sprite31e225.png) 0 -270px;
    width: 34px;
    height: 34px;
    vertical-align: middle;
    display: inline-block;
  }

  .ngdialog,.ngdialog *,.ngdialog :before,.ngdialog :after {
    box-sizing: border-box
  }

  .ngdialog {
    position: fixed;
    overflow: auto;
    z-index: 10000;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
  }

  .ngdialog-overlay {
    position: fixed;
    background: rgba(0,0,0,.4);
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    backface-visibility: hidden;
    animation: ngdialog-fadein .1s;
  }

  @keyframes ngdialog-fadein {
    0% {
      opacity: 0
    }

    100% {
      opacity: 1
    }
  }

  .ngdialog-content {
    background: #fff;
    backface-visibility: hidden;
    animation: ngdialog-fadein .1s;
  }

  .ngdialog.default .ngdialog-content {
    background: #fff;
    border-radius: 5px;
    color: #444;
    margin: 0 auto;
    max-width: 100%;
    padding: 1em;
    position: relative;
    width: 450px;
  }

  .create_chatroom_dlg.ngdialog.default .ngdialog-content {
    padding: 0;
    border-radius: 4px;
    overflow: hidden;
    width: 480px;
    height: 600px;
  }

  .create_chatroom_dlg .dialog_hd .title {
    margin-bottom: 0;
    line-height: 50px;
    background-color: #f9f9f9;
  }

  .dialog_hd .title {
    font-size: 16px;
    font-weight: 400;
    text-align: center;
    margin-bottom: 14px;
  }

  .nav_tabs {
    text-align: center;
    font-size: 0;
    border-radius: 3px;
    border-bottom: 1px solid #e5e5e5;
    background-color: #f9f9f9;
  }

  .nav_tab {
    display: inline-block;
    *display: inline;
    *zoom: 1;
    line-height: 37px;
    cursor: pointer;
    color: #288525;
    margin-left: 20px;
    margin-right: 20px;
    font-size: 14px;
    position: relative;
  }

  .nav_tab.selected:before {
    content: "";
    display: block;
    position: absolute;
    width: 0;
    height: 0;
    left: 50%;
    margin-left: -3px;
    bottom: 0;
    border: 7px solid transparent;
    border-bottom-color: #e5e5e5;
  }

  .nav_tab.selected:after {
    content: "";
    display: block;
    position: absolute;
    width: 0;
    height: 0;
    left: 50%;
    margin-left: -3px;
    bottom: -1px;
    border: 7px solid transparent;
    border-bottom-color: #fff;
  }

  .selector {
    max-height: 110px;
    overflow-y: auto;
    position: absolute;
    min-height: 60px;
    left: 0;
    right: 0;
    z-index: 999;
    background-color: #fff;
    padding: 10px 0 0 20px;
  }

  .selector:after {
    content: "";
    visibility: hidden;
    display: block;
    height: 0;
    clear: both;
  }

  .selector .input_box {
    margin-bottom: 10px;
    position: relative;
    float: left;
  }

  .selector .input_box .input {
    border: 0;
    font-size: 16px;
    width: 5em;
    line-height: 40px;
    height: 40px;
  }

  .addchat_searchicon {
    background: url(../images/sprite31e225.png) 0 0;
    width: 30px;
    height: 30px;
    vertical-align: middle;
    display: inline-block;
  }

  .chooser {
    margin: 0 20px;
    padding-top: 60px;
  }

  .create_chatroom_dlg .dialog_ft {
    padding-top: 20px;
  }

  .dialog_ft {
    border-top: 1px solid #f1f1f1;
    overflow: hidden;
    text-align: center;
    font-size: 0;
    padding-top: 10px;
  }

  .create_chatroom_dlg .dialog_ft .button_default {
    display: inline-block;
    border: 1px solid #c1c1c1;
    background-color: #c9c9c9;
    font-size: 14px;
    width: 190px;
    text-align: center;
    line-height: 40px;
    color: #fff;
    text-decoration: none;
    border-radius: 4px;

  }

  .create_chatroom_dlg .dialog_ft .button_primary {
    display: inline-block;
    background-color: #3caf36;
    font-size: 14px;
    width: 190px;
    text-align: center;
    line-height: 42px;
    color: #fff;
    text-decoration: none;
    border-radius: 4px;
  }

  .ngdialog.default .ngdialog-content .ngdialog-close {
    top: 14px;
    right: 14px;
    background: url(../images/sprite31e225.png) 0 -41px;
    width: 25px;
    height: 25px;
    vertical-align: middle;
    display: inline-block;
  }

  .ngdialog.default .ngdialog-close {
    border-radius: 5px;
    cursor: pointer;
    position: absolute;
    right: 0;
    top: 0;
  }

  .nav_view {
    position: absolute;
    top: 154px;
    right: 0;
    bottom: 0;
    left: 0;
  }

  h1 {
    color: #fff;
  }

  .panel_wrapper {
    height: 100%;
  }

  .profile_mini_wrap {
    z-index: 999!important;
    border-radius: 4px;
    overflow: hidden;
    box-shadow: rgba(0,0,0,.1) 0 5px 10px;
  }

  .scale-fade {
    transition: all 0 cubic-bezier(0.25,.46,.45,.94);
    transition-timing-function: cubic-bezier(0.25,.46,.45,.94)
  }

  .profile_mini_hd .avatar .img {
    width: 220px;
    height: 220px;
    display: block;
  }

  .profile_mini_bd {
    padding: 20px;
    min-height: 74px;
  }

  .profile_mini_bd .nickname_area {
    margin-bottom: 8px;
  }

  .profile_mini_bd .nickname {
    font-weight: 400;
    font-size: 18px;
    display: inline-block;
    vertical-align: middle;
    max-width: 110px;
    width: auto;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    word-wrap: normal;
  }

  .profile_mini_bd .meta_item {
    overflow: hidden;
  }

  .profile_mini_bd .meta_item .label {
    float: left;
    font-size: 12px;
    color: #888;
  }

  .profile_mini_bd .meta_item .value {
    font-size: 12px;
    color: #888;
    width: 120px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    word-wrap: normal;
  }

  .scale-fade-enter {
    transform: scale(0.7);
    transition-duration: 150ms;
    opacity: 0;
  }

  .scale-fade-enter-active {
    transform: scale(1);
    opacity: 1;
  }

  .scale-fade-leave {
    transform: scale(1);
    transition-duration: 150ms;
    opacity: 1;
  }

  .scale-fade-leave-active {
    transform: scale(0.7);
    opacity: 0;
  }

  .overlay_mask {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: 10;
    cursor: pointer;
  }
</style>
