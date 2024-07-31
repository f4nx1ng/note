### soot

Soot 是 McGill 大学的 Sable 研究小组自 1996 年开始开发的 Java 字节码分析工具，它提供了多种字节码分析和变换功能。通过它可以进行过程内和过程间的分析优化，以及程序流图的生成；还能通过图形化的方式输出，让用户对程序有个直观的了解。尤其是做单元测试的时候，可以很方便的通过这个生成控制流图然后进行测试用例的覆盖，显著提高效率。

#### JDD当中的soot选项配置及解读

总体来说，没有什么特别的地方，基本上就是一些常规的处理。比较值得注意的就是关于方法的一些额外设置，比如不要丢弃方法体，探查所有的可达方法等等。

```java
Options.v().set_src_prec(Options.src_prec_class);//指定文件的输入类型
Options.v().set_process_dir(Collections.singletonList(((inputClassPath))));//指定输入目录，并处理所有dir中发现的类
Options.v().set_allow_phantom_refs(true);//允许虚类存在，找不到对应源代码的类就被称为虚类
Options.v().set_whole_program(true);//启动全程序分析
Options.v().set_prepend_classpath(true);//将新类路径添加到现有类路径前面，确保新类可以被优先处理
Options.v().set_keep_line_number(true);//保留源代码当中的行号信息，以便于生成分析报告和调试信息
Options.v().set_output_format(Options.output_format_jimple);//定义输出形式为jimple
Options.v().set_output_dir(RegularConfig.outputDir+"/JimpleOutput/framework1");//定义输出目录
Options.v().set_drop_bodies_after_load(false);//于控制在加载字节码文件后是否丢弃方法体
// Options.v().set_no_bodies_for_excluded(true);
Options.v().setPhaseOption("cg", "all-reachable:true");//这个选项会告诉soot，构建调用图的时候考虑所有可达方法，而不仅仅是主方法
```

#### Soot的基本数据结构

soot有一系列很复杂的层次结构，比如Scene，SootClass，SootField，SootMethod，Body，Local，Trap，Unit；

Sence：表示整个分析所发生的运行环境，通过Scene可以设置提供给soot分析的所有的应用类、包含main方法的类和有关过程间分析的访问信息。

SootClass：简单的来说就是被分析的Class在soot当中的表示形式，它包含一个Java类所有相关的信息，例如这个类的类名、修饰符、父类、SootField、SootMethod链等

SootField：表示类的属性，也可以叫做域。

SootMethod：表示一个类中的单个方法。

Body：Body用于表示方法的具体实现，在soot中，一个Body隶属于一个SootMethod，即soot用一个Body为一个方法存储代码。分析应用时可以依赖于Body来访问方法当中的信息（局部变量、方法体等等）

Chains：

#### 参考文献及内容

有关soot

https://xz.aliyun.com/t/11643

https://fynch3r.github.io/soot%E7%9F%A5%E8%AF%86%E7%82%B9%E6%95%B4%E7%90%86/

https://www.brics.dk/SootGuide/sootsurvivorsguide.pdf

https://github.com/SugarP1g/Learning-Program-analysis

有关tai-e及静态代码分析

https://y4er.com/posts/simple-use-of-the-java-static-analysis-framework-tai-e/#%E6%96%87%E6%9C%AB

tai-e的文字版教程及配套习题https://tai-e.pascal-lab.net/lectures.html

b站课程https://www.bilibili.com/video/BV1b7411K7P4