# 性能优化

Form [Front-end-Developer-Interview-Questions](https://github.com/h5bp/Front-end-Developer-Interview-Questions)

* 你会用什么工具来查找代码中的性能问题？
* 你会用什么方式来增强网站的页面滚动效能？
* 请解释 layout、painting 和 compositing 的区别。

Form [Front-end-Developer-Interview-Questions](https://github.com/h5bp/Front-end-Developer-Interview-Questions)



- [Yahoo Best Practices for Speeding Up Your Web Site](https://developer.yahoo.com/performance/rules.html)
- [Google 使用 RAIL 模型评估性能](https://developers.google.com/web/fundamentals/performance/rail)
- [前端性能优化最佳实践](https://csspod.com/frontend-performance-best-practices/)


1. 前端长列表的性能优化

只渲染页面用用户能看到的部分。并且在不断滚动的过程中去除不在屏幕中的元素，不再渲染，从而实现高性能的列表渲染。
借鉴着这个想法，我们思考一下。当列表不断往下拉时，web中的dom元素就越多，即使这些dom元素已经离开了这个屏幕，不被用户所看到了，这些dom元素依然存在在那里。导致浏览器在渲染时需要不断去考虑这些dom元素的存在，
造成web浏览器的长列表渲染非常低效。因此，实现的做法就是捕捉scroll事件，当dom离开屏幕，用户不再看到时，就将其移出dom tree。

