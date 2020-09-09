## Maven的构建

#### 一、Maven的声明周期（lifecycle）

首先maven提供了三种生命周期（default，site，clean）

- default：定义了一系列的阶段（complie，test，package），当执行到某一个阶段时，前面的都要依次执行，默认的阶段，上面是什么都没有的。如果某一阶段上绑定了目标（goal），就去执行它（在executions中），如果多个目标，就依次按照顺序来执行



#### 二、Maven的阶段（phase）



#### 三、Maven的插件（plugin）

在build/plugins中定义插件

#### 四、Maven的目标（goal）