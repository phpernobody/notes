JAVA单元测试
1. 下载junit-*.jar并引入工程
2. 创建UnitTestBase类，完成对Spring配置文件的加载、销毁
3. 所有的单元测试类都继承自UnitTestBase，通过它的getBean方法获取想要得到的对象
4. 子类(具体执行单元测试的类)加注解：`@RunWith(BlockJUnit4ClassRunner.class)`
5. 单元测试方法加注解：`@test`
6. 右键选择要执行的单元测试方法执行或执行一个类的全部单元测试方法