<template>
  <div id="gitalk-container"></div>
</template>

<script>
export default {
  name: 'blog-comments',
  mounted() {
    const commentConfig = {
      // 是否开启
      enable: true,
      // clientID
      clientID: '46535a54f0518cb77c09',
      // clientSecret
      clientSecret: '334b24a4731fa5e3f8288b6aacc2b3acf31cdd4e',
      // 评论项目名
      repo: 'md-docs',
      owner: 'filway',
      admin: ['filway'],
      githubID: 'filway',
      id: decodeURI(window.location.pathname),
      language: 'zh-CN',
      distractionFreeMode: true,
    }
    const gitalk = new Gitalk(commentConfig)
    gitalk.render('gitalk-container')
  },
}
</script>