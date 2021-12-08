## 1.2
- ```cout << "test" << endl;```等效为```(cout << "test") << endl;```。由于 ```<<```返回其左侧的运算对象，也就是std::ostream对象（即cout）。因此endl还是可以通过```<<```赋给std::ostream对象。综上即```<<```可以连续输出的原因。```>>```同理，也可以连续赋值。
- ```cout```的信息存储在memory中，除非碰到有flush作用的命令（如```endl```）才会flush memory，将目前为止在内存中的数据显示出来。也正是因此，在debug时候，```cout```的内容一定要加```endl```，否则很有可能程序crash了，却没有显示相应的信息，而导致无法准确定位crash位置。
- reader相比于过时的document更加相信作者的comments，因此确保更新代码时候，同时更新注释。
