<template>
  <div>
    <div id="header">
      <div class="header-left fl">
        <ul>
          <li class="logo fl">
            <a href="#"
              ><span class="logo-img"></span><span>网易云音乐</span></a
            >
          </li>
          <li class="fl">
            <div class="search">
              <div class="fl">
                <input type="text" placeholder="搜索音乐，视频，歌词，电台" />
              </div>
            </div>
          </li>
        </ul>
      </div>
      <div class="header-right">
        <div class="user">
          <ul>
            <li class="fl user-inform">
              <span class="avatar"></span><span class="logn-state">未登录</span
              ><span class="selection-inform"></span>
            </li>
            <li class="fl VIP-apply">开通VIP</li>
            <li class="fl setting">设置</li>
          </ul>
        </div>
        <div class="controls">
          <li class="mini-btn fl">最小视窗</li>
          <li class="min-btn fl">最小化</li>
          <li class="max-btn fl">最大化</li>
          <li class="quit-btn fl">关闭</li>
        </div>
      </div>
    </div>
    <div id="palyer">
      <div class="player-w">
        <div class="album-cover fl">
          <div class="album"><span class="albumcover song-img"></span></div>
          <div class="userbtn">
            <div class="fl"><i class="fa fa-heart-o"></i>喜欢</div>
            <div class="fl"><i class="fa fa-plus-square-o"></i>收藏</div>
            <div class="fl"><i class="fa fa-download"></i>VIP下载</div>
            <div class="fl"><i class="fa fa-share-square-o"></i>分享</div>
          </div>
        </div>
        <div class="lyric">
          <div class="song">
            <span class="song-name"></span>
            <span class="song-mv icon_sprite"></span>
            <span class="song-tone icon_sprite"></span>
          </div>
          <div class="song-info">
            <span class="song-album"><a href="#"></a></span>
            <span class="singer"><a href="#"></a></span>
            <span class="song-sour"><a href="#"></a></span>
          </div>
          <div class="song-lyric"><div class="song-lyric-box"></div></div>
        </div>
        <div class="bgc-img song-img"></div>
      </div>
    </div>
    <div id="player-control">
      <span class="playerIcon songPrev"></span>
      <span class="playerIcon songStop" id="myplay"></span>
      <span class="playerIcon songNext"></span>
      <span class="song-time-state">00:00</span>
      <div class="songState" style="width: 400px">
        <div class="songBarNow" style="width: 0%"></div>
        <span class="songBarDot" style="left: -4px"></span>
      </div>
      <span class="song-end-state">00:00</span>
      <span class="playerIcon songCycleOrder" id="mycycle"></span>
      <span class="playerIcon playerControlButtomOn" id="mylyricOndesk"></span>
    </div>
    <div class="songLyricOnDesk">
      <p></p>
      <p></p>
    </div>
    <div>
      <audio src="" id="myaudio"></audio>
    </div>
  </div>
</template>
<script>
export default {
  head: {
    title: '音乐播放器',
    script: [
      {
        src: 'https://cdnjs.cloudflare.com/ajax/libs/jquery/1.1./jquery.min.js',
      },
      {
        src: '/assets/audio/asset.js',
      },
      {
        src: '/assets/audio/index.js',
      },
    ],
    link: [
      {
        rel: 'icon',
        href: 'favicon.ico',
      },
      {
        rel: 'stylesheet',
        href: '/assets/audio/index.css',
      },
      {
        rel: 'stylesheet',
        href: '/assets/audio/reset.css',
      },
    ],
  },
}
</script>
<style lang="stylus"></style>
