# 关于Error: Component series.scatter3D not exists. Load it first的解决方案

# 问题所在

在vue引入echarts中没包含3d的依赖包，所以你需要引入3d依赖解析包
 npm install echarts-gl
 echarts-gl就是用来解析3d的

此时你还需要在页面中引入这个包
 import ‘echarts-gl’
 在你vue的页面引入即可！

如有其他问题欢迎讨论