# md
md
- (void)blockText {
    
    //1 __NSGlobalBlock__  全局block   存储在代码区（存储方法或者函数）
    void(^myBlock1)() = ^() {
        NSLog(@"我是老大");
    };
    
    NSLog(@"b1%@",myBlock1);
    
    //2 __NSStackBlock__  栈block  存储在栈区
    //block内部访问外部变量
    //block的本质是一个结构体
    int n = 5;
    void(^myBlock2)() = ^() {
        NSLog(@"我是老二%d", n);
    };
    NSLog(@"b2%@", myBlock2);
    
    
    //3 __NSMallocBlock__  堆block 存储在堆区  对栈block做一次copy操作
    void(^myBlock3)() = ^() {
        NSLog(@"我是老二%d", n);
    };
    NSLog(@"b3%@", [myBlock3 copy]);
    
    
    /*
     由以上三个例子可以看出当block没有访问外界的变量时,是存储在代码区,
     当block访问外界变量时时存储在栈区, 而此时的block出了作用域就会被释放
     以下示例:
     */
    [self test];//当此代码结束时,test函数中的所有存储在栈区的变量都会被系统释放, 因此如果属性的block是用assign修饰时  当再次访问时就会出现野指针访问.
    self.myblock();
    
    
}

- (void)test {
    int n = 5;
    [self setMyblock:^{
        NSLog(@"%d",n);
    }];
    NSLog(@"test--%@",self.myblock);
    
}
@end


//2017-02-15 13:46:59.873 Proty[1807:552088] b1<__NSGlobalBlock__: 0x103c4e170>
//2017-02-15 13:46:59.874 Proty[1807:552088] b2<__NSMallocBlock__: 0x61800005a2e0>
//2017-02-15 13:46:59.874 Proty[1807:552088] b3<__NSMallocBlock__: 0x61800005a3a0>
//2017-02-15 13:46:59.874 Proty[1807:552088] test--<__NSMallocBlock__: 0x61800005a340>
//2017-02-15 13:46:59.874 Proty[1807:552088] 5
