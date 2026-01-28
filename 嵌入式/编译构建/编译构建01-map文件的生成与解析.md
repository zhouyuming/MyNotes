# 什么是map文件？
map文件是编译器链接后生成的文本文件，用于反映程序符号、代码段（RO）、数据段（RW/ZI）在Flash/RAM中的映射关系。

通过分析map文件（如Keil生成的），可以分析系统内存占用、查看函数调用关系、查找冗余代码以及定位硬件崩溃位置。

# map文件核心组成与分析要点
- Section Cross References（交叉引用）：分析函数间的调用关系，即哪个函数调用了哪个函数，可用于追踪代码流向。
- Removing Unused input sections（删除冗余节）：显示被编译器优化掉的未调用函数和变量，帮助理解代码删除情况。
- Image Symbol Table（符号表）：详细列出所有符号（变量、函数）的地址、大小和所在文件，是内存分析的核心部分。
- Memory Map（内存分布）：直观展示RO（只读代码/数据）、RW（读写数据）、ZI（零初始化数据）在存储器中的分布。

# 关键数据含义解释
- RO-code/RO-data：代码段和只读常量（如const变量），存储在Flash。
- RW-data：已初始化的全局变量，存储在RAM。
- ZI-data：未初始化或初始化为0的全局变量，存储在RAM，程序启动时自动清零。
- 程序实际占用RAM：RW-data + ZI-data。

# 常用场景
- 分析内存溢出：若程序运行时奔溃，查看Memory Map区，检查RW/ZI是否超过RAM限制。
- 查找大函数/变量：通过符号表按大小排序，找出占用Flash或RAM过多的函数或数组。
- 代码优化：检查Removing Unused段，确认冗余代码被成功删除。
- 调试死机：根据硬件错误追踪到的地址，在Map文件中查找对应函数。 

# map文件分析工具
https://gitcode.com/open-source-toolkit/92b73

# 参考链接
https://www.cnblogs.com/chengeputongren/p/12177423.html
