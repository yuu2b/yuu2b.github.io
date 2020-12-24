---
title: about
date: 2020-10-12 20:19:36
keywords: 关于
description: 
comments: false
photos: https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/banner/about.jpg
---
{% raw %}
<!-- 因为vue和botui更新导至bug,现将对话移至js下的botui中配置 -->
<div class="entry-content">
  <div class="moe-mashiro" style="text-align:center; font-size: 50px; margin-bottom: 20px;">[Sleeplessness]</div>
  <div id="hello-mashiro" class="popcontainer" style="min-height: 300px; padding: 2px 6px 4px; background-color: rgba(242, 242, 242, 0.5); border-radius: 10px;">
    <center>
    <p>
    </p>
    <h4>
    与&nbsp;<ruby>
    宇宇&nbsp;<rp>
    （</rp>
    <rt>
     </rt>
    <rp>
    ）</rp>
    </ruby>
    对话中...</h4>
    <p>
    </p>
    </center>
    <bot-ui></bot-ui>
  </div>
</div>
<script src="https://cdn.jsdelivr.net/gh/yuu2b/cdn/js/botui.js"></script>
<script>
bot_ui_ini()
</script>
{% endraw %}