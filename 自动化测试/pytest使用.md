1.命令行直接输入pytest之后，会自动执行当前目录下所有已test_开头的.py文件里的test_开头的方法

2.case执行顺序的问题：

使用pip install pytest-ordering插件，然后配置case的order值即可

比如只配置了test_pipline1这个case的order值为1时，命令行运行【pytest -v】时的运行顺序为

可以看到，只把配置了order=1的test_pipline1提出来了，而TestPipline这个py文件中另一个test_pipline2还在它应该的顺序上



```python
@pytest.mark.run(order=1)
    def test_pipline1(self):
        print('test_pipline1')
      
# 命令行运行【pytest -v】时的运行顺序为 
testcases/TestTask/test_pipline.py::TestPipline::test_pipline1 PASSED
testcases/TestAutolink/test_component.py::TestComponent::test_component1 PASSED
testcases/TestAutolink/test_component.py::TestComponent::test_component2 PASSED
testcases/TestAutolink/test_module.py::TestModule::test_module1 PASSED
testcases/TestAutolink/test_module.py::TestModule::test_module2 PASSED
testcases/TestTask/test_pipline.py::TestPipline::test_pipline2 PASSED
testcases/TestTask/test_task.py::TestTask::test_task1 PASSED
testcases/TestTask/test_task.py::TestTask::test_task2 PASSED   
```

```python
# 如果不配置test_pipline1的order的话，运行顺序为：
testcases/TestAutolink/test_component.py::TestComponent::test_component1 PASSED
testcases/TestAutolink/test_component.py::TestComponent::test_component2 PASSED
testcases/TestAutolink/test_module.py::TestModule::test_module1 PASSED
testcases/TestAutolink/test_module.py::TestModule::test_module2 PASSED
testcases/TestTask/test_pipline.py::TestPipline::test_pipline1 PASSED
testcases/TestTask/test_pipline.py::TestPipline::test_pipline2 PASSED
testcases/TestTask/test_task.py::TestTask::test_task1 PASSED
testcases/TestTask/test_task.py::TestTask::test_task2 PASSED 
```



3.scope不适合login的这种全局的最开始的case设置，如果把login写出下面的fixture的话，那在需要和这个login有前后关系的case（比如下面的test_pipline2）的参数上都要加login参数，如果case很多，几百条的话那太不优雅了

```python
@pytest.fixture(scope='session')
def login():
    pass
  
class TestPipline(object):
    def test_pipline1(self):
        print('*test_pipline1')

    def test_pipline2(self,login):
        print('*test_pipline2')
```

所以，我觉得scope适合同一个py文件中case的依赖关系设置，依赖的case数不是很多的情况

这里引申一下，@pytest.fixture(scope='session') 也可以放到conftest.py文件中，这个文件时pytest独特的文件，命名必须为conftest.py

如果确实有一些初始化什么的特殊的操作，可以用conftest.py+fixture来做到统一管理初始化的方法，但是如果知识简单的依赖，直接写在test py文件里即可，还很易读







