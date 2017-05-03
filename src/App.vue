<template>
    <div id="app">
        <style-edit ref="styleEditor" :code="currentStyle" :curCode="compCode"></style-edit>
        <RsumeEdit :markdown="currentMarkdown" :enableHtml="html"></RsumeEdit>
    </div>
</template>

<script>
    import  StyleEdit from './components/StyleEdit.vue'
    import  RsumeEdit from './components/RsumeEdit.vue'
    export default {
        name: 'app',
        data () {
            return {
                html: false,
                currentStyle: '',
                compCode: '',
                fullStyle: [
                    `/*
* Inspired by http://strml.net/
* 大家好，我是方方
* 二月了，好多公司都在招聘，你是不是也在准备简历呀。
* 说做就做，我也来写一份简历！
*/
/* 首先给所有元素加上过渡效果 */
* {
  -webkit-transition: all .3s;
  transition: all .3s;
}
/* 白色背景太单调了，我们来点背景 */
html {
  color: rgb(222,222,222); background: rgb(0,43,54);
}
/* 文字离边框太近了 */
.styleEditor {
  padding: .5em;
  border: 1px solid;
  margin: .5em;
  overflow: auto;
  width: 45vw; height: 90vh;
}
/* 代码高亮 */
.hljs-comment{ color:#988989 ;}
.hljs-selector-class,.hljs-selector-tag{ color: rgb(133,153,0); }
.hljs-attribute{ color: rgb(187,137,0); }
.hljs-built_in{ color: yellow; }
.hljs-number{ color: rgb(42,161,152); }
/* 加点 3D 效果呗 */
html{
  -webkit-perspective: 1000px;
          perspective: 1000px;
}
.styleEditor {
  position: fixed; left: 0; top: 0;
  -webkit-transition: none;
  transition: none;
  -webkit-transform: rotateY(10deg) translateZ(-100px) ;
          transform: rotateY(10deg) translateZ(-100px) ;
}
/* 接下来我给自己准备一个编辑器 */
.resumeEditor{
  position: fixed; right: 0; top: 0;
  padding: .5em;  margin: .5em;
  width: 48vw; height: 90vh;
  border: 1px solid;
  background: white; color: #222;
  overflow: auto;
}
/* 好了，我开始写简历了 */
`,
                    `
/* 这个简历好像差点什么
 * 对了，这是 Markdown 格式的，我需要变成对 HR 更友好的格式
 * 简单，用开源工具翻译成 HTML 就行了
 */
`
                    ,
                    `
/* 再对 HTML 加点样式 */
.resumeEditor{
padding: 2em;
}
.resumeEditor h2{
display: inline-block;
border-bottom: 1px solid;
margin: 1em 0 .5em;
}
.resumeEditor ul,.resumeEditor ol{
list-style: none;
}
.resumeEditor ul> li::before{
content: '•';
margin-right: .5em;
}
.resumeEditor ol {
counter-reset: section;
}
.resumeEditor ol li::before {
counter-increment: section;
content: counters(section, ".") " ";
margin-right: .5em;
}
.resumeEditor blockquote {
margin: 1em;
padding: .5em;
background: #ddd;
}
`
                ],
                currentMarkdown: '',
                fullMarkdown: `方应杭
----
资深前端工程师，资深前端讲师，现在在 [饥人谷](http://jirengu.com) 教前端课程。
技能
----
* 前端开发
* Rails 开发
* Node.js 开发
* 前端授课

工作经历
----
1. [饥人谷](http://jirengu.com)
2. 腾讯即时通讯平台部
3. 阿里巴巴B2B部门
4. 彩程知人项目组

链接
----
* [GitHub](https://github.com/frankfang)
* [我的文章](https://www.zhihu.com/people/zhihusucks/pins/posts)
> 如果你喜欢这个效果，Fork [我的项目](https://github.com/jirengu-inc/animating-resume)，打造你自己的简历！
`

            }
        },
        components: {
            StyleEdit,
            RsumeEdit
        },
        mounted () {
            this.intStyle();
        },
        methods: {
            intStyle: async function () {
                await  this.showStyle(0)
                await  this.showResume()
                await  this.showStyle(1)
                await  this.enableHtml()
                await  this.showStyle(2)
            },
            showStyle (num) {
                return new Promise((resolve, reject) => {
                    var n = 0;
                    let nowPre = this.fullStyle.filter((_, index) => {
                        return index < num
                    }).reduce((p, c) => {
                        return p + c
                    }, '')


                    let thePre = this.fullStyle[num - 1] || ''
                    var timer = setInterval(() => {
                        n = n + 1;
                        this.$refs.styleEditor.goBottom()

                        this.currentStyle = nowPre + this.fullStyle[num].substring(0, n);
                        if (this.currentStyle.substring(n - 1, n) === ';') {
                            this.compCode = this.currentStyle;
                        }
                        if (n > this.fullStyle[num].length - 1) {
                            resolve();
                            clearInterval(timer)
                        }
                    }, 0)
                })
            },
            showResume () {
                return new Promise((resolve, reject) => {
                    var n = 0;
                    var timer = setInterval(() => {
                        n = n + 1;
                        this.currentMarkdown = this.fullMarkdown.substring(0, n);
                        if (n > this.fullMarkdown.length - 1) {
                            resolve()
                            clearInterval(timer)
                        }
                    }, 10)
                })
            },
            enableHtml () {
                this.html = true;
            }
        }

    }
</script>

<style>
    *{margin: 0; padding: 0;}
    *{box-sizing: border-box;}
    *::before{box-sizing: border-box;}
    *::after{box-sizing: border-box;}
</style>
